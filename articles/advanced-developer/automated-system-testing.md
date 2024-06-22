<!-- Filename: J4.x:Automated_System_Testing / Display title: Automated System Testing -->

<span id="main-portal-heading">GSoC 2018  
Automated System Testing for Joomla 4  
Documentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Automated System Testing Package For Joomla

### Abstract

This article is regarding system test (codeception tests) for Joomla 4.

### Installation

#### For Linux

- **Open a session and change to the document root of your local
  webserver.**
    $ cd /var/www/html/
    /var/www/html$
```bash
$ cd /var/www/html/
/var/www/html$
```
- **Clone the current joomla repository into your webserver root
  folder**
    /var/www/html$ git clone git@github.com:joomla/joomla-cms.git
    Clone into 'joomla-cms' ...
```bash
/var/www/html$ git clone git@github.com:joomla/joomla-cms.git
Clone into 'joomla-cms' ...
```

Are you new with github? Here you can find informations about setting it
up: <a href="https://help.github.com/articles/set-up-git/"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://help.github.com/articles/set-up-git/</a>
If you get an error you can try
`git clone `<a href="https://github.com/joomla/joomla-cms.git" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>https://github.com/joomla/joomla-cms.git</code></a>
instead of `git clone git@github.com:joomla/joomla-cms.git`

- **Change to the directory joomla-cms and make sure that you are on the
  4.0-dev branch**

Change to the directory:

```bash
    /var/www/html$ cd joomla-cms
    /var/www/html/joomla-cms$
```

Make sure you are on the correct branch:

```bash
    /var/www/html/joomla-cms$ git branch
    * staging
    /var/www/html/joomla-cms$ git fetch origin 4.0-dev:4.0-dev
    /var/www/html/joomla-cms$ git checkout 4.0-dev
    /var/www/html/joomla-cms$ git branch
    * 4.0-dev 
     staging
```

- **Your joomla-cms folder should contain this files:**

```bash
    /var/www/html/joomla-cms$ ls
    acceptance.suite.yml  cli                 Gemfile       jenkins-phpunit.xml  modules            RoboFile.php          web.config.txt
    administrator         codeception.yml     htaccess.txt  karma.conf.js        package.json       robots.txt.dist
    appveyor-phpunit.xml  components          images        language             phpunit.xml.dist   scss-lint.yml
    build                 composer.json       includes      layouts              plugins            templates
    build.js              composer.lock       index.php     libraries            README.md          tests
    build.xml             dev                 installation  LICENSE.txt          README.txt         tmp
    cache                 drone-package.json  Jenkinsfile   media                RoboFile.dist.ini  travisci-phpunit.xml
```

- **Optional: Have a look into composer.json for information what
  software you will install via composer.**

```bash
    /var/www/html/joomla-cms$ cat composer.json
```

Read more about \[how to install
composer\](<a href="https://getcomposer.org/doc/00-intro.md" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://getcomposer.org/doc/00-intro.md</a>)
here.

- **Install via composer**

```bash
    /var/www/html/joomla-cms$ composer install
```

Loading composer repositories with package information Installing
dependencies (including require-dev) from lock file Package operations:
63 installs, 0 updates, 0 removals

```bash
    - Installing behat/gherkin (v4.4.5): Downloading (100%)
    - Installing sebastian/recursion-context (3.0.0): Downloading (100%)
    - Installing joomla/test-system (dev-master ca3879f): Cloning ca3879f603
    Generating optimized autoload files
```

Now you see this repo in the folder
`/var/www/html/joomla-cms/libraries/vendor/joomla/test-system`

- **Optional: Prepare the database**

If you use MySQL or PostgreSQL as database and your user has create
database privileges the Database is automatically created by the Joomla
installer. But the safest way is to create the database before running
Joomla's web installer.

```bash
    /var/www/html/joomla-cms$ mysql -u root -p
```

```
    mysql> create database test_joomla;
    Query OK, 1 row affected (0,00 sec)
    mysql> quit;
    Bye
```

- **Update the file acceptance.suite.yml to your needs.**

Important are the options

```
    - url in the section JoomlaBrowser and Helper\Acceptance,
    - database name and
    - database user.
    - database password.

    /var/www/html/joomla-cms$ cat acceptance.suite.yml

class_name: AcceptanceTester modules:

       enabled:
           - Asserts
           - JoomlaBrowser
           - Helper\Acceptance
       config:
           JoomlaBrowser:
               url: 'http://localhost/joomla-cms/test-install'     # the url that points to the joomla installation at /tests/system/joomla-cms
               browser: chrome
               window_size: 1280x1024
               username: 'admin'                      # UserName for the Administrator
               password: 'admin'                      # Password for the Administrator
               database host: 'localhost'             # place where the Application is Hosted #server Address
               database user: 'root'                  # MySQL Server user ID, usually root
               database password: 'YOURDBPASSWORD'    # MySQL Server password, usually empty or root
               database name: 'test_joomla'           # DB Name, at the Server
               database type: 'mysqli'                # type in lowercase one of the options: MySQL\MySQLi\PDO
               database prefix: 'jos_'                # DB Prefix for tables
               install sample data: 'no'              # Do you want to Download the Sample Data Along with Joomla Installation, then keep it Yes
               sample data: 'Default English (GB) Sample Data'    # Default Sample Data
               admin email: 'admin@mydomain.com'      # email Id of the Admin
               language: 'English (United Kingdom)'   # Language in which you want the Application to be Installed
           Helper\Acceptance:
               url: 'http://localhost/joomla-cms/test-install' # the url that points to the joomla installation at /tests/system/joomla-cms - we need it twice here
               MicrosoftEdgeInsiders: false             # set this to true, if you are on Windows Insiders
```

error_level: "E_ALL & ~E_STRICT & ~E_DEPRECATED"

- **Copy the file `acceptance.suite.yml` to this repo.**

Currently we need the configuation file twice!

```bash
    /var/www/html/joomla-cms$ cp acceptance.suite.yml ./libraries/vendor/joomla/test-system/src
```

- **Optional: Create and edit the file RoboFile.ini.**

```bash
    /var/www/html/joomla-cms$ cat RoboFile.ini
```

If you want to setup your test website (document root) in a different
folder, you can do that here. You can also set an absolute path, i.e. /path/to/my/cms/folder

cmsPath = test-install

(Linux / Mac only) If you want to set a different owner for the CMS root
folder, you can set it here.

localUser =

- **Optional: Set use owner of the project to your user.**

Change the username/usergroup of the files by traveling the directories
recursively. This is made possible by the ‘-R’ option.

```bash
    /var/www/html/joomla-cms$sudo chown -R username:usergroup /var/www/html/joomla-cms
```

### Running - Ready! Run the first tests

#### For Linux

- **Using Robo**
```bash
    /var/www/html/joomla-cms$ libraries/vendor/bin/robo run:tests➜  Running tests
    [Filesystem\DeleteDir] Deleted test-install...
    [Filesystem\CopyDir] Copied from ./media to test-install/media
    [Filesystem\CopyDir] Copied from ./nbproject to test-install/nbproject
    [Filesystem\CopyDir] Copied from ./images to test-install/images
    [Filesystem\CopyDir] Copied from ./libraries to test-install/libraries
```
  
Now a browser window opens and you can see the tests running in it.

If you get an error like this

```bash
    /var/www/html/joomla-cms$ libraries/vendor/bin/robo run:tests
```

➜ Running tests

```
    [Filesystem\CopyDir] Copied from ./media to test-install/media
    [Filesystem\CopyDir] Copied from ./nbproject to test-install/nbproject
    [Filesystem\CopyDir] Copied from ./images to test-install/images
    [error]  Failed to copy "./libraries/vendor/consolidation/robo/scenarios/symfony2/tests" because file does not exist.
```

you have to delete two files:

```
    /var/www/html/joomla-cms$ rm ./libraries/vendor/consolidation/robo/scenarios/symfony2/tests
    /var/www/html/joomla-cms$ rm ./libraries/vendor/consolidation/robo/scenarios/symfony4/tests
```

The tests use Codeception Testing Framework, if you want to know more
about the technology used for testing please check: \[Testing Joomla
Extensions with Codeception\](<a
href="https://docs.joomla.org/Testing_Joomla_Extensions_with_Codeception"
class="external free" target="_blank"
rel="noreferrer noopener">https://docs.joomla.org/Testing_Joomla_Extensions_with_Codeception</a>).

- **Using Codeception**

1\. Install a selenium-standalone-server using the following link :
<a href="https://www.seleniumhq.org/download/" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://www.seleniumhq.org/download/</a>.
After downloading the file selenium-server-standalone-#.jar, run it
using the command
```bash
    java -jar selenium-server-standalone-#.jar
```
2\. To run all the tests,
```bash
    /var/www/html/joomlaMain$ ./libraries/vendor/bin/codecept run
```
3\. To run individual tests. For example, you want to test MenuCest file
in com_menu use the following command :
```bash
    /var/www/html/joomlaMain$ ./libraries/vendor/bin/codecept run acceptance /administrator/components/com_menu/MenuCest 
```
