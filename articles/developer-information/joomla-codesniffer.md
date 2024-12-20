<!-- Filename: Joomla_CodeSniffer / Display title: Coding Standards -->

<div class="alert alert-warning">
The latter part of this article needs to be updated!
</div>

## An AI Overview

Coding standards are important for software development because they:

- **Improve code quality**
  - Coding standards help ensure that code is reliable, secure, and safe. They can also help reduce performance issues and security concerns that might arise from poor coding practices.
- **Make code more readable and maintainable**
  - Coding standards help make code easier to understand, read, and maintain. This can also make it easier for new developers to work with the code.
- **Speed up development**
  - Coding standards can help developers avoid common mistakes that can slow down coding processes.
- **Improve collaboration**
  - Coding standards can help facilitate collaboration among developers, even in large teams.
- **Ensure consistency**
  - Coding standards help ensure that code is consistent across projects. This can make it easier for developers to work together on the same projects.
- **Improve scalability**
  - Coding standards can help ensure that code can scale without becoming unmanageable.
- **Provide clear criteria for code reviews**
  - Coding standards can help provide clear criteria for code reviews, which can lead to more effective feedback.

Coding standards often include rules for indentation, line length, brace placement, and spacing.

Joomla uses the [PSR-12](https://www.php-fig.org/psr/psr-12/) coding standard. You can enable this coding standard in your IDE and get hints if you're not following the coding standard or use an auto fix, too. We recommend that you follow this standard when developing your own extensions to stay compatible with core and to ensure your code will hopefully work with future versions.

## PHP Code Sniffer

Please consult the current [PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer/) source for information on this utility and how to install it. Be aware that the original source is abandoned but it may be listed by search engines. There are two scripts:

- **phpcs** detects violations of a coding standard and produces a report.
- **phpcbf** attempts to correct coding standards violations and produces a report on what it has done.

You can install the individual scripts or use composer to install them. You can also find them in a Joomla clone located in libraries/vendor/bin along with some other useful utilities.

## Testing PHP Code Sniffer

If you put a global installation in your `$PATH` you can run the code sniffer from the command line in the root of a project. Try this to display a list of coding standards:

```sh
phpcs -i
The installed coding standards are MySource, PEAR, PSR1, PSR2, PSR12, Squiz and Zend
```

As an example the following command was used in an older project folder that used the former Joomla custom coding standard. Amongst other things it used tabs rather than spaces for layout. Many similar lines have been omitted below for brevity.

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | [ ] A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The
     |         |     first symbol is defined on line 22 and the first side effect is on line 10.
   1 | ERROR   | [x] Header blocks must be separated by a single blank line
  22 | ERROR   | [ ] Each class must be in a namespace of at least one level (a top-level vendor name)
  24 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  25 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  26 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
  ...
  42 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  48 | ERROR   | [x] No space found after comma in argument list
  48 | ERROR   | [x] Expected 1 space after closing parenthesis; found newline
...
  67 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
  75 | ERROR   | [x] Space after opening parenthesis of function call prohibited
  75 | ERROR   | [x] Expected 0 spaces before closing parenthesis; 1 found
...
 138 | WARNING | [ ] Line exceeds 120 characters; contains 168 characters
 139 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 139 | WARNING | [ ] Line exceeds 120 characters; contains 141 characters
...
 156 | ERROR   | [x] Spaces must be used to indent lines; tabs are not allowed
 157 | ERROR   | [x] Expected 1 newline at end of file; 0 found
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 131 MARKED SNIFF VIOLATIONS AUTOMATICALLY
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 123ms; Memory: 8MB
```

The previous example project contained a single php file and no css or js files. phpcs produces a report for each php, css and js file in a project. Here are some examples for css and js files:

```sh
FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/mediacat.css
----------------------------------------------------------------------------------
FOUND 33 ERRORS AFFECTING 32 LINES
----------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 51 | ERROR | [x] Whitespace found at end of line
...
 79 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
----------------------------------------------------------------------------------
PHPCBF CAN FIX THE 33 MARKED SNIFF VIOLATIONS AUTOMATICALLY
----------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/css/file-icon-classic.min.css
-----------------------------------------------------------------------------------------------
FOUND 0 ERRORS AND 1 WARNING AFFECTING 1 LINE
-----------------------------------------------------------------------------------------------
 1 | WARNING | File appears to be minified and cannot be processed
-----------------------------------------------------------------------------------------------

FILE: /Users/ceford/git/j4x-demos-com-mediacat/com_mediacat/media/js/mediacat-site.js
-------------------------------------------------------------------------------------
FOUND 38 ERRORS AFFECTING 30 LINES
-------------------------------------------------------------------------------------
  3 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
  5 | ERROR | [x] Expected 1 space after FUNCTION keyword; 0 found
  6 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
...
 13 | ERROR | [x] Opening brace should be on a new line
...
 29 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
 29 | ERROR | [x] Expected at least 1 space before "+"; 0 found
 29 | ERROR | [x] Expected at least 1 space after "+"; 0 found
...
 36 | ERROR | [x] Spaces must be used to indent lines; tabs are not allowed
-------------------------------------------------------------------------------------
PHPCBF CAN FIX THE 38 MARKED SNIFF VIOLATIONS AUTOMATICALLY
-------------------------------------------------------------------------------------
```
## Fixing Violations

The single php file that `FOUND 132 ERRORS AND 3 WARNINGS AFFECTING 116 LINES` shown above can be mostly fixed as follows:

```sh
phpcbf --standard=PSR12 .

PHPCBF RESULT SUMMARY
-----------------------------------------------------------------------------------
FILE                                                               FIXED  REMAINING
-----------------------------------------------------------------------------------
/Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php     131    4
-----------------------------------------------------------------------------------
A TOTAL OF 131 ERRORS WERE FIXED IN 1 FILE
-----------------------------------------------------------------------------------

Time: 232ms; Memory: 8MB
```

Running the phpcs utility again shows which problems remain:

```sh
phpcs --standard=PSR12 .

FILE: /Users/ceford/git/j4xdemos-plg-toc/j4xdemostoc/j4xdemostoc.php
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
FOUND 1 ERROR AND 3 WARNINGS AFFECTING 4 LINES
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   1 | WARNING | A file should declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it should execute logic with side effects, but should not do both. The first
     |         | symbol is defined on line 23 and the first side effect is on line 11.
  23 | ERROR   | Each class must be in a namespace of at least one level (a top-level vendor name)
 132 | WARNING | Line exceeds 120 characters; contains 168 characters
 133 | WARNING | Line exceeds 120 characters; contains 141 characters
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Time: 127ms; Memory: 8MB
```
Conclusion: an extension coded for an earlier Joomla version and using different coding standard now needs some work!

## Installation in IDEs

Most Integrated Development Environments can use the code sniffer as you work or when you save a file.

### Installation in VSCode

In VSCode (and/or VScodium), select the Extensions icon, search for PHP_CodeSniffer and install it. It needs configuration:

1. In **Settings** find PHP_CodeSniffer
2. Set the **PHP Code Sniffer → Exec:** field to suit the location of your phpcs binary.
3. In the **PHP Code Sniffer: Standard** list select **PSR12**.

Close and you are ready for action.

Some of the problems shown above can be fixed with PSR1 directives.

```php
<?php

/**
 * @package     Whatever
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

use joomla\CMS\...

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

Others need some thought!

After installation in VSCode, code style violations will be detected and marked with a red squiggle underline. Hover over the problem to see a message and potential solution, as in this example:

```txt
Header blocks must be separated by a single blank line
PHP_CodeSniffer(PSR12.Files.FileHeader.SpacingAfterBlock)
```

<div class="alert alert-warning">
The following sections need to be updated!
</div>

### Installation in PhpStorm

Code Sniffer is supported out of the box in PhpStorm. Go to Settings and under **Editor → Inspections** you will see the list of sniffers you have installed.

#### Set Path to Code Sniffer

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Languages & Frameworks
3.  Click on PHP
4.  Click on Quality Tools
5.  Click on PHP cs fixer dropdown arrow
6.  The configuration is set to Local by default
7.  Click on the 3 dots behind it to open the configuration screen
8.  The first option is the PHP Code Sniffer (phpcs) path
9.  Click on the 3 dots behind the path to select the location of the phpcs file. See above on where phpcs may be installed on your site
10. Click on Validate to make sure the path is correct and phpcs is working
11. Click OK

#### Activating the Code Style

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Editor
3.  Click on Inspections
4.  In the list, go to PHP
5.  Click on PHP Code Sniffer Validation
6.  Click on the check box behind it to activate it
7.  Click the Reload button (2 arrows) to force a reload of rules from disk
8.  Joomla should now be available in the list. See following image.
9.  Click OK

![CodeSniffer in PHPStorm](../../../en/images/getting-started/core-phpstorm-code-sniffer.png)

#### Netbeans

Netbeans has the sniffer functionality integrated into the core system.

1.  Start your Netbeans IDE
2.  Open **Tools → Options → PHP → Code Analysis → Code Sniffer**
3.  You have to set the path to *phpcs.bat* from the installed PHP_CodeSniffer PEAR package
    - For Unix based systems the path is something like /usr/bin/phpcs
    - In XAMPP (windows) you can find the file in the PHP root folder (e.g. C:\xampp\php\phpcs.bat)
4.  As "Default Standard," choose "Joomla" to use the Joomla! standard
5.  Now you can click *OK* to start sniffing
6.  Open from the top menu **Source → Inspect...**
7.  Enjoy

### Installation in Eclipse

1. **Help → Install New Software...**
2. **Work with** Fill in one of the update site URLs.
3. Select the desired tools.
4. Restart Eclipse.
5. **Window → Preferences**
6. **PHP Tools → PHP CodeSniffer**

![Eclipse PTI settings](../../../en/images/getting-started/core-eclipse-pti-settings.png)

You are now able to sniff for code violations against common standards.

![Codesniffer in Eclipse](../../../en/images/getting-started/core-eclipse-pti.png)

### Installation in Geany

- Open a PHP file. (Otherwise the build menu is not accessible.)
- On the top menu, select **Build → Set Build Commands**.
- Select the second field and name it as you wish. Enter this code in the Command:<br>
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Enter this code in the Error Regular Expression field:  `(.+) [|]\s+([0-9]+)`
- Select OK.
- If the Message Window is not open, display it by selecting it in the top View menu.
- When viewing any PHP file, press F9 to see the errors found.

### Installation in Atom

- Install phpcs and the Joomla standards as described above.
- In **Atom → Preferences → Install** install **linter-phpcs** and all its requirements.
- In **Atom → Preferences → Packages → linter-phpcs → Settings** adjust
  - **Executable Path** to the path of your phpcs
  - **Code Standard Or Config File**: PSR12
  - **Tab Width**: 4
