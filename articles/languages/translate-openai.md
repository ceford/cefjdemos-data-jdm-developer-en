<!-- Filename: J4.x:Developer:_Required_Software / Display title: .ini File Translation Example -->

## Introduction

This article presents an example of a method to translate a language pack using openai.com. It seemed necessary to use this method to fulfil a requirement for a language pack in Scottish Gaelic without the availability of Gaelic speaking translators. 

## Preparation

The default English language pack contains .ini files for three clients: administrator (454 files), api (2 files) and site (69 files).

The command line PHP script used for translation is shown in full below. It requires two local git repositories for translation processing purposes:

- A clean Joomla 5 installation with the `5.3-dev` branch checked out: use a clone of the Joomla CMS without building it.
- A place to receive the translations with a branch named `joomla5` checked out: use `path/yourprefix-pkg-lc-cc` where"
    - `path` is a suitable path to a place for local repositories.
    - `yourprefix` is whatever you use for a personal namespace.
    - `pkg` is literally to indicate this is a package.
    - `lc` is a two-character ISO language code.
    - `cc` is a two character ISO country code.

The script checks these locations and aborts if either is not set correctly. It processes each English .ini file in turn, breaks it into chunks and sends each chunk to openai.com for translation. The translated chunks are then reassembled and output to a new language .ini file.

The script outputs the name of each file being processed with progress indicated by dots. Example input and output:

```
php initrans.php
Client (one of api, admin or site): api
Translating api/language/en-GB/com_media.ini. Chunks (total): .
Translating api/language/en-GB/joomla.ini. Chunks (total): ..........
Total = 2
```

Where `total` indicates the number of chunks the file has been split into and the dots (...) indicate the chunk being processed.

If something goes wrong there is a message to that effect and the script terminates. Partial translations of a single file are saved so that translation can continue where it previously ceased when the script is re-started. 

Processing the `com_media.ini` file takes only a few seconds as it contains just a few key-value strings. Processing the `joomla.ini` file takes a few minutes as it contains about 900 strings. The api client is used as an example because it has only two .ini files and takes a few **minutes** to complete. The administrator and site clients contain many more .ini files and may take **hours** to complete!


Some parameters are hard-coded:

- The path to a Joomla 5 installation on the computer running this script.
- The path to location to receive translated .ini files.
- The language code of the target language.
- The language name of the target language.

Not hard coded:

- Your personal openai.com api key is read from an environment variable.

## The initrans.php file

```php
<?php

/**
 * Class to translate Joomla core .ini files into other languages using openai.com.
 * This example is for Scottish Gaelic.
 *
 * The class variables are used for configuration. Edit them to suit the site and target language.
 * The $base_in and $base_out folders must exist.
 */
class ChatGPTIniTranslate {

    /**
     * Base path for ini files in an existing Joomla installation.
     */
    protected $base_in = '/Users/ceford/Sites/jclean5/';

    /**
     * Base path for translated ini files, typically a local git repo.
     */
    protected $base_out = '/Users/ceford/git/cefjdemos-pkg-gd-gb/';

    /**
     * Language code used in path settings.
     */
    protected $language_code = 'gd-GB';

    /**
     * Language name passed to openai.com.
     */
    protected $language_name = 'Scottish Gaelic';

    /**
     * URL of current version of the API endpoint.
     */
    protected $open_ai_url = 'https://api.openai.com/v1/chat/completions';

    /**
     * Personal openai api key: https://platform.openai.com/account/api-keys
     * Use an environment variable set from the command line or shell config. Example:
     * export OPENAI_API_KEY="your key here"
     */
    protected $openai_api_key;

    protected $model = 'gpt-4o';                    // change if needed
    protected $maxCharsPerChunk = 3500;             // ~2-4KB safe starting point (tweak as needed)
    protected $pauseBetweenCallsMs = 300;           // polite pause between requests
    protected $maxRetries = 6;                      // exponential backoff attempts
    protected $progressDir = __DIR__ . '/progress'; // where partial responses are saved

    /**
     * Process one of the client ini folders
     *
     * @param   string  $folder The name of the folder: api, admin or site
     *
     * @return void
     */
    public function go($folder) {
        echo "\nTranslating {$this->language_name}\n\n";

        $this->openai_api_key = getenv('OPENAI_API_KEY');

        if (!$this->openai_api_key) {
            exit("\API key not set in environment variable.\n");
        }

        switch ($folder) {
            case 'api':
                $source = "api/language/en-GB/";
                $sink   = "{$this->language_code}/api_{$this->language_code}/";
                break;
            case 'admin':
                $source = "administrator/language/en-GB/";
                $sink   = "{$this->language_code}/admin_{$this->language_code}/";
                break;
            case 'site':
                $source  = "language/en-GB/";
                $sink   = "{$this->language_code}/site_{$this->language_code}/";
                break;
            default:
                exit("Unknown folder: {$folder}\n");
        }
        $this->ensureCorrectGitBranch($this->base_in, $source, '5.3-dev');
        $this->ensureCorrectGitBranch($this->base_out, $sink, 'joomla5');

        // Get a list of ini files in the English source
        $files = glob($this->base_in . $source . '*.ini');

        foreach ($files as $file) {
            $rel = basename($file);
            $outFile = $this->base_out . $sink . $rel;

            // If the translation has been done, skip this file.
            if (is_file($outFile)) {
                continue;
            }
            try {
                $sourceFile = $this->base_in . $source . $rel;
                $this->translateIniFile($sourceFile, $outFile);
            } catch (Exception $e) {
                echo "\nError translating $rel: " . $e->getMessage() . "\n";
                // decide whether to continue or abort:
                // continue;
                exit(1);
            }
        }

        echo "\n\nAll done.\n";
    }

    /**
     * Check that the source or sink folders exist and are git repos with the joomla5 branch checked out.
     *
     * @param string $repoPath The absolute path to the folder containing a .git folder.
     * @param string $folder The subfolder path where ini files are located.
     * @param string $expectedBranch Should be set to j5.3-dev for input and joomla5 for output
     */
    protected function ensureCorrectGitBranch(string $repoPath, string $folder, string $expectedBranch): void {
        if (!is_dir($repoPath . $folder)) {
            exit("\nThe ini folder does not exist: {$repoPath}{$folder}\n\n");
        }
        if (!is_dir($repoPath . '/.git')) {
            exit("Not a Git repository: $repoPath");
        }

        // Run git command to get current branch
        $cmd = "cd " . escapeshellarg($repoPath) . " && git rev-parse --abbrev-ref HEAD";
        exec($cmd, $output, $exitCode);

        if ($exitCode !== 0) {
            exit("Failed to determine current Git branch in $repoPath");
        }

        $currentBranch = trim($output[0]);

        if ($currentBranch !== $expectedBranch) {
            exit("Expected branch '$expectedBranch', but found '$currentBranch' in $repoPath. Aborting.");
        }

        // Optional: confirm up-to-date with remote
        echo "✓ Git branch check passed: $currentBranch in {$repoPath}{$folder}\n";
    }

    /**
     * Minimal sanity check: ensure keys are unchanged between original and translated lines.
     * Returns array [ok=>bool, message=>string]
    */
    function sanity_check_chunk(array $originalLines, array $translatedLines): array {
        // Strip blank and comment lines and map keys in order
        $origKeys = [];
        foreach ($originalLines as $ln) {
            $ln = trim($ln);
            if ($ln === '' || $ln[0] === ';') continue;
            if (preg_match('/^([A-Z0-9_\.]+)=/', $ln, $m)) {
                $origKeys[] = $m[1];
            }
        }
        $transKeys = [];
        foreach ($translatedLines as $ln) {
            $ln = trim($ln);
            if ($ln === '' || $ln[0] === ';') {
                continue;
            }
            if (preg_match('/^([A-Z0-9_\.]+)=/', $ln, $m)) {
                $transKeys[] = $m[1];
            }
        }
        if (count($origKeys) !== count($transKeys)) {
            return ['ok' => false, 'message' => 'Key count mismatch'];
        }
        for ($i = 0; $i < count($origKeys); $i++) {
            if ($origKeys[$i] !== $transKeys[$i]) {
                return ['ok' => false, 'message' => "Key mismatch: {$origKeys[$i]} != {$transKeys[$i]} at index $i"];
            }
        }
        return ['ok' => true, 'message' => 'ok'];
    }

    // ---------- CHUNKING ----------
    protected function chunkIniByCharLimit(string $filePath, int $maxChars): array {
        $lines = file($filePath, FILE_IGNORE_NEW_LINES);

        // Remove first 5 lines (4 copyright + 1 blank)
        $lines = array_slice($lines, 5);

        $chunks = [];
        $current = [];
        $currentLen = 0;

        foreach ($lines as $ln) {
            $lnWithNewline = $ln . "\n";
            $len = mb_strlen($lnWithNewline, '8bit');
            // if this single line > maxChars, still put it alone in chunk
            if ($currentLen + $len > $maxChars && count($current) > 0) {
                $chunks[] = $current;
                $current = [];
                $currentLen = 0;
            }
            $current[] = $ln;
            $currentLen += $len;
        }
        if (count($current) > 0) $chunks[] = $current;
        return $chunks;
    }

    // ---------- MAIN PROCESS for one file ----------
    protected function translateIniFile(string $sourceFile, string $outFile) {
        if (!is_dir($this->progressDir)) {
            mkdir($this->progressDir, 0777, true);
        }
        $chunks = $this->chunkIniByCharLimit($sourceFile, $this->maxCharsPerChunk);
        $total = count($chunks);
        echo "\nTranslating {$sourceFile}. Chunks ({$total}): ";
        $baseName = basename($sourceFile);
        $progressFile = "$this->progressDir/$baseName.progress.json";

        // try to resume: load already translated chunks if progress file present
        $translatedChunks = [];
        if (file_exists($progressFile)) {
            $translatedChunks = json_decode(file_get_contents($progressFile), true) ?? [];
        }

        for ($i = 0; $i < $total; $i++) {
            if (isset($translatedChunks[$i])) {
                echo "Chunk ".($i+1)."/$total already done (resume)\n";
                continue;
            }

            $chunkLines = $chunks[$i];
            $stringLines = implode("\n", $chunkLines);

            $messages = [
                [
                    'role' => 'system',
                    'content' => <<<SYS
You are translating Joomla .ini language files.

RULES:
- Preserve the exact format: each line is KEY="value".
- Only translate the text inside the quotes.
- Do not remove or alter the equals sign, the quotes, or the key names.
- Do not add extra spaces.
- Do not change the order of lines.
- Do not wrap the output in code blocks, markdown fences, or any extra formatting.
- Do not add explanations or comments.
- Preserve comment lines (starting with ";") exactly as they are, without translation.
SYS
                ],
                [
                    'role' => 'user',
                    'content' => <<<USER
Translate the following Joomla .ini content from English to {$this->language_name}.

Input:
{$stringLines}

Output must be plain text. No markdown. No extra characters before or after the .ini lines.
USER
                ]
            ];

            // Progress indicator
            //echo "Translating chunk ".($i+1)."/$total ...\n";
            echo ".";

            $rawTranslation = $this->callOpenAI($messages, $this->maxRetries);

            // Normalize result into lines
            $translatedLines = preg_split("/\r\n|\n|\r/", trim($rawTranslation));

            // Sanity-check keys unchanged
            $check = $this->sanity_check_chunk($chunkLines, $translatedLines);
            if (!$check['ok']) {
                // Save the raw translation for inspection and throw
                file_put_contents("$progressFile.err_$i.txt", $rawTranslation);
                throw new RuntimeException("Sanity check failed for chunk ".($i+1).": " . $check['message'] . ". Raw response saved.");
            }

            // Save translated chunk to progress array and file
            $translatedChunks[$i] = $translatedLines;
            file_put_contents($progressFile, json_encode($translatedChunks, JSON_UNESCAPED_UNICODE|JSON_PRETTY_PRINT));

            // be polite
            usleep($this->pauseBetweenCallsMs * 1000);
        }

        // Merge chunks and write final file
        $outLines = [];
        for ($i = 0; $i < $total; $i++) {
            $outLines = array_merge($outLines, $translatedChunks[$i]);
        }

        // final check: ensure utf-8
        $outText = implode("\n", $outLines) . "\n";
        if (!mb_check_encoding($outText, 'UTF-8')) {
            throw new RuntimeException("Output is not valid UTF-8");
        }

        // When saving, prepend your custom copyright block
        $year = date("Y");
        $newCopyright = "; Joomla! Project. Translation by Clifford E Ford using openai.com.\n" .
        "; (C) {$year} Open Source Matters, Inc. <https://www.joomla.org>\n" .
        "; License GNU General Public License version 2 or later; see LICENSE.txt\n" .
        "; Note : All ini files need to be saved as UTF-8\n\n";

        file_put_contents($outFile, $newCopyright . $outText);
        // done, remove progress file
        @unlink($progressFile);
        //echo "\nTranslated file written: $outFile\n";
    }

    protected function callOpenAI(array $messages, int $maxRetries = 3) {
        $headers = [
            "Content-Type: application/json",
            "Authorization: Bearer " . $this->openai_api_key
        ];
        $data = [
            'model' => $this->model,
            'messages' => $messages,
            'temperature' => 0,
            'top_p' => 1,
        ];

        $attempt = 0;
        while ($attempt < $maxRetries) {
            $attempt++;

            $ch = curl_init($this->open_ai_url);
            curl_setopt_array($ch, [
                CURLOPT_POST => true,
                CURLOPT_HTTPHEADER => $headers,
                CURLOPT_POSTFIELDS => json_encode($data),
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_CONNECTTIMEOUT => 10,
                CURLOPT_TIMEOUT => 60
            ]);

            $result = curl_exec($ch);
            $httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
            $curlErr = curl_error($ch);
            curl_close($ch);

            if ($result !== false && $httpCode >= 200 && $httpCode < 300) {
                $response = json_decode($result, true);

                // support both Chat Completions and Responses-style: check typical path
                if (isset($response['choices'][0]['message']['content'])) {
                    return $response['choices'][0]['message']['content'];
                } elseif (isset($response['output'][0]['content'][0]['text'])) {
                    // alternative shape
                    return $response['output'][0]['content'][0]['text'];
                } else {
                    // unknown shape but return raw
                    echo "Bad response!\n";
                }
            }

            error_log("Attempt $attempt failed: HTTP $httpCode $curlErr");
            sleep(2 * $attempt); // backoff
        }

        throw new Exception("Failed after $maxRetries attempts");
    }
}

$client = readline("Client (one of api, admin or site): ");
if ($client === 'api' || $client === 'admin' || $client === 'site') {
    $chat = new ChatGPTIniTranslate;
    $chat->go($client);
} else {
    echo "Unrecognised client: {$client}. Please start again.\n";
}
```
