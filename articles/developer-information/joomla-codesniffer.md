<!-- Filename: Joomla_CodeSniffer / Display title: Joomla CodeSniffer -->

## A Nose For Joomla

This is a custom coding standard for the PHP CodeSniffer that attempts
to codify and enforce the Joomla coding standards. This article covers
how to set the automatic code style checker. It consists of three steps:

1.  Install PHP CodeSniffer (phpcs).
2.  Clone the Joomla Code Style (for use with for phpcs).
3.  Configure your IDE to work with PHP CodeSniffer and Joomla Code
    Style.

<img
src="https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/300px-CodeSnifferInAction.jpg"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/450px-CodeSnifferInAction.jpg 1.5x, https://docs.joomla.org/images/thumb/e/e4/CodeSnifferInAction.jpg/600px-CodeSnifferInAction.jpg 2x"
data-file-width="995" data-file-height="641" width="300" height="193" />
<a href="https://docs.joomla.org/File:CodeSnifferInAction.jpg"
class="internal" title="Enlarge"></a>PHP Code Sniffer in action

## Why?

- Coherent and consistent coding practice makes the files look more
  professional. Conflicting styles in the same project (or worse, the
  same file) not only look sloppy, they encourage further sloppiness.
- When all code complies with the same standard, bad code is easier for
  everyone to spot.
- It makes it easier for someone new to a particular file in the project
  to find and fix errors or extend functionality.
- If there is no consistent standard maintained, sometimes developers
  will reformat the code to suit themselves. This causes a wide range of
  changes in the code repository. If there is a later problem, a
  significant change could be lost in the chaff produced by a diff.

## 1. Installation of PHP Code Sniffer (phpcs)

### Composer

PHP Code Sniffer is available via Composer. It can be installed
system-wide with the following command:  
`composer global require --dev squizlabs/php_codesniffer "~3.5"` On
**Linux** PHP Code Sniffer (phpcs) will be installed under your user
folder:

- Symbolic Link: `~/.composer/vendor/bin/phpcs`
- The file: `~/.composer/vendor/squizlabs/php_codesniffer/scripts/phpcs`

To create that symbolic link, use on the command line:
` $ sudo ln -s ~/.config/composer/vendor/squizlabs/php_codesniffer/scripts /usr/bin/phpcs `

or (for example OpenSuse)

- Symbolic Link: `~/.config/composer/vendor/bin/phpcs`
- The file:
  `~/.config/composer/vendor/squizlabs/php_codesniffer/scripts/phpcs`

On **Windows** PHP Code Sniffer (phpcs) will be installed under your
user folder:

- Symbolic Link:
  `c:\Users\Username\AppData\Roaming\Composer\vendor\bin\phpcs`
- The file:
  `c:\Users\Username\AppData\Roaming\Composer\vendor\squizlabs\php_codesniffer\scripts\phpcs`

### Add composer bin directory to your \$PATH

In order to execute PHP Code sniffer from anywhere on Linux or Mac
command line you have to add `.composer/vendor/bin` to your \$PATH
variable. To do this persistent edit your shells start file, like your
.profile or .bash_profile file depending which shell you are using (echo
\$SHELL shows you which one you are using).

Add the following line to the file:
`export PATH=$PATH:~/.composer/vendor/bin`

Alternatively you can also create a symbolic link in the bin folder:
`ln -s ~/.composer/vendor/bin/phpcs /usr/local/bin/phpcs`

## 2. Install Joomla Coding Standards

In order to have PHP Code Sniffer (phpcs) sniffing your Joomla projects
for the right code style, you have to install the Joomla Coding
Standards.

Using composer we can execute:
`composer global require --dev joomla/coding-standards "~3.0@alpha"`

Notice that it says `@alpha`, that is because the coding standard is
still in alpha state and we need to allow the installation of alpha
status code. If we do not include the @alpha the coding standards will
not install and throw this error.

    Your requirements could not be resolved to an installable set of packages.

      Problem 1
        - The requested package joomla/coding-standards ~3.0 is satisfiable by joomla/coding-standards[3.0.0-alpha, 3.0.0-alpha2, 3.x-dev] but these conflict with your requirements or minimum-stability.

Once the installation is done the coding standard will be installed in
the global composer folder. The location of this folder depends on your
operating system. Some popular locations are:

**Mac / Linux**

    ~/.composer/vendor/joomla/coding-standards/

    ~/.config/composer/vendor/joomla/coding-standards/

**Windows**

    c:\Users\Username\AppData\Roaming\Composer\vendor\joomla\coding-standards\

This is using Composer on Windows.

If you can't find it in any of the above folders, note that the folder
you are looking for ends with **\joomla\coding-standards\\** Finally we
need to tell Code Sniffer that the Joomla coding standards exist.

**Install Joomla Coding Standards**

1.  Check if certain paths are already set by running
    `phpcs --config-show`
2.  You can get an answer that looks like this:
    `installed_paths: /path/to/installation`
3.  In step 2, if you see the **installed_paths** you need to copy that
4.  Set the Joomla Coding Standards path in phpcs by executing
    `phpcs --config-set installed_paths /Users/user/.composer/vendor/joomla/coding-standards`.
    If you copied any paths in step 2, include them here as well by
    separating them with a comma. The command looks like
    `phpcs --config-set installed_paths [/to/path1],[/to/path2],[/Users/user/.composer/vendor/joomla/coding-standards]`
5.  Verify the path is set correctly by running `phpcs --config-show`
6.  Verify that phpcs can see the Joomla codestyle by running `phpcs -i`
7.  The output should look like this:
    `The installed coding standards are MySource, PEAR, PHPCS, PSR1, PSR2, Squiz, Zend and Joomla`

## Usage

You invoke the custom standard by
`phpcs --standard=Joomla file/to/sniff`

To test a platform file using the provided platform coding standards,
use `phpcs --standard=build/phpcs/Joomla path/to/file/or/folder`

Further documentation on the use of phpcs can be found at:
<a href="http://pear.php.net/package/PHP_CodeSniffer/docs"
class="external autonumber" target="_blank"
rel="nofollow noreferrer noopener">[1]</a>

## 3. IDE Integration

Everybody loves the console. It is, with no doubt, the most effective
way to do whatever you need to do. Sometimes even Linux gurus need a
little bit of comfort.

Fortunately there is a plug-in available for PhpStorm, Eclipse and
Netbeans that integrates the CodeSniffer into your favourite IDE, so any
coding standard violations are shown like "normal" errors.

### PhpStorm

Code Sniffer is supported out of the box in PhpStorm. Go to Settings and
under Editor  **→**  Inspections you will see the list of sniffers you
have installed.

#### Set Path to Code Sniffer

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Languages & Frameworks
3.  Click on PHP
4.  Click on Quality Tools
5.  Click on PHP cs fixer dropdown arrow
6.  The configuration is set to Local by default
7.  Click on the 3 dots behind it to open the configuration screen
8.  The first option is the PHP Code Sniffer (phpcs) path
9.  Click on the 3 dots behind the path to select the location of the
    phpcs file. See above on where phpcs may be installed on your site
10. Click on Validate to make sure the path is correct and phpcs is
    working
11. Click OK

##### Activating the Joomla Code Style

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Editor
3.  Click on Inspections
4.  In the list, go to PHP
5.  Click on PHP Code Sniffer Validation
6.  Click on the check box behind it to activate it
7.  Click the Reload button (2 arrows) to force a reload of rules from
    disk
8.  Joomla should now be available in the list. See following image:<img
    src="https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/500px-PhpStorm8CodeSniffer.png"
    decoding="async"
    srcset="https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/750px-PhpStorm8CodeSniffer.png 1.5x, https://docs.joomla.org/images/thumb/9/9c/PhpStorm8CodeSniffer.png/1000px-PhpStorm8CodeSniffer.png 2x"
    data-file-width="2446" data-file-height="1624" width="500" height="332"
    alt="PHPStorm CodeSniffer" />
9.  Click OK

#### PHP PSR-0, PSR-1 and PSR-2

- <a
  href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md</a>
- <a
  href="https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-1-basic.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-1-basic.md</a>
- <a
  href="https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-2-advanced.md"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/pmjones/fig-standards/blob/psr-1-style-guide/proposed/PSR-2-advanced.md</a>

##### Using PHP PSR-1 and PSR-2 Sniff

A CodeSniffer sniff to check against the PSR-x Coding Standard can be
used as well. In that case you can select a different standard than
Joomla.

##### Using Joomla Code style

It's nice to be able to check that the standards are respected. It's
even nicer if PhpStorm helps you format properly too as you are coding.
To use the Joomla code style in PhpStorm

1.  Open Settings (CTRL-ALT-S / CMD-,)
2.  Go to Editor
3.  Click on Code Style
4.  Select Joomla from the Scheme select box

#### PhpStorm / Alternative Method

**NOTE**: This method is **outdated**, but it might be useful if you are
looking for a different integration. It also demonstrates the use of
external tools in PhpStorm - so it *shouldn't be deleted*.

The Code Sniffer can also be integrated easily as an external tool.
PhpStorm will display the output in the console, including click-able
links containing line and column numbers to the files that contain
errors.

- Click on "Settings" and search for "External tools"
- Click "Add..."
- Choose a "Name", "Group" and "Description".
- Click "Output Filters"
  - Click "Add...", Choose a name and enter under "Regular expression to
    match output" the value: `$FILE_PATH$:$LINE$:$COLUMN$`
- "Program": Search for the phpcs executable on your system. You have to
  set the path to the *phpcs.bat* from the installed PHP_CodeSniffer
  PEAR package
  - For Unix based systems, the path is something like /usr/bin/phpcs
  - In XAMPP (windows), you can find the file in the PHP root folder
    (e.g. C:\xampp\php\phpcs.bat)
- "Parameters":
  - `--standard=/build/phpcs/Joomla` The path to the Joomla! coding
    standards.
  - `--report=emacs` The will generate a simple list containing links to
    the error files
  - Optionally you may want to specify `-p` for "progress" or `-n` for
    "errors only".
  - The last parameter has to be `$FilePath$` specifying the file or
    folder you want to sniff.

A typical "Parameters" line on a Linux system might look like this:

    -np --standard=/home/elkuku/libs/joomla/build/phpcs/Joomla --report=emacs $FilePath$

You may now right click any file or folder and choose the sniffer from
the context menu or add a new tool bar button with a nice Joomla! logo
<img src="https://docs.joomla.org/images/6/67/Icon-16-joomla.png"
decoding="async" data-file-width="16" data-file-height="16" width="16"
height="16" alt="Icon-16-joomla.png" />.

### Netbeans

Netbeans has the sniffer functionality integrated into the core system.

1.  Start your Netbeans IDE
2.  Open Tools =\> Options =\> PHP =\> Code Analysis =\> Code Sniffer
3.  You have to set the path to *phpcs.bat* from the installed
    PHP_CodeSniffer PEAR package
    - For Unix based systems the path is something like /usr/bin/phpcs
    - In XAMPP (windows) you can find the file in the PHP root folder
      (e.g. C:\xampp\php\phpcs.bat)
4.  As "Default Standard," choose "Joomla" to use the Joomla! standard
5.  Now you can click *OK* to start sniffing
6.  Open from the top menu Source =\> Inspect...
7.  Enjoy

### Eclipse

<img
src="https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/200px-Eclipse_pti.png"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/300px-Eclipse_pti.png 1.5x, https://docs.joomla.org/images/thumb/4/44/Eclipse_pti.png/400px-Eclipse_pti.png 2x"
data-file-width="1388" data-file-height="762" width="200"
height="110" />
<a href="https://docs.joomla.org/File:Eclipse_pti.png" class="internal"
title="Enlarge"></a>1) Eclipse PTI

Installation is a breeze and follows the usual pattern:

1.  `Help => Install New Software...`
2.  `Work with:` Fill in one of the update site URLs found here:
    <a href="http://www.phpsrc.org/eclipse/pti/" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener">http://www.phpsrc.org/eclipse/pti/</a>
3.  Select the desired tools
4.  Restart Eclipse.

<img
src="https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/150px-Eclipse_pti_settings.png"
class="thumbimage" decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/225px-Eclipse_pti_settings.png 1.5x, https://docs.joomla.org/images/thumb/a/a1/Eclipse_pti_settings.png/300px-Eclipse_pti_settings.png 2x"
data-file-width="973" data-file-height="819" width="150" height="126" />
<a href="https://docs.joomla.org/File:Eclipse_pti_settings.png"
class="internal" title="Enlarge"></a>2) Eclipse PTI settings

You are now able to sniff for code violations against common standards
like PEAR or Zend etc.

To sniff against your own standards, specify their location and activate
them (see screen shot 2)

1.  `Window => Preferences`
2.  `PHP Tools => PHP CodeSniffer`

Happy sniffing.

### Geany

- Open a PHP file. (Otherwise the build menu is not accessible.) See
  this <a
  href="http://static.xscreenshot.com/2016/07/13/08/screen_6bc0692cf995702a1e379b39643d0c2d"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>
- On the top menu, select Build-\>Set Build Commands.
- Select the second field and name it as you wish. Enter this code in
  the Command:
  `phpcs --standard=Joomla "%f" | sed -e 's/^/%f |/' | egrep 'WARNING|ERROR'`
- Enter this code in the Error Regular Expression field:
  `(.+) [|]\s+([0-9]+)` See this
  <a href="http://view.xscreenshot.com/ef00820cf7c017ce659c8e9f0b02d3ae"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>
- Select OK.
- If the Message Window is not open, display it by selecting it in the
  top View menu.
- When viewing any PHP file, press F9 to see the errors found.
  <a href="http://view.xscreenshot.com/8704b9fd1bda3f841364a0ffa28a54ae"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Screenshot</a>

### Atom

- Install phpcs and the Joomla standards as described above.
- In **Atom \> Preferences \> Install** install **linter-phpcs** and all
  its requirements.
- In **Atom \> Preferences \> Packages \> linter-phpcs \> Settings**
  adjust
  - **Executable Path** to the path of your phpcs
  - **Code Standard Or Config File**: Joomla
  - **Tab Width**: 4

### References

- <a href="https://github.com/joomla/coding-standards"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/coding-standards</a> -
  Joomla! Coding Standards
- <a href="http://www.phpsrc.org/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">http://www.phpsrc.org/</a> PTI -
  PHP tools integration for Eclipse
- <a href="http://sourceforge.net/projects/phpmdnb/" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">http://sourceforge.net/projects/phpmdnb/</a>
  Netbeans plugin
- <a
  href="http://hakre.wordpress.com/2010/03/06/php-code-sniffer-eclipse-and-wordpress/"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">http://hakre.wordpress.com/2010/03/06/php-code-sniffer-eclipse-and-wordpress/</a> -
  Excellent article. Just change "Wordpress" for "Joomla!" ;)
