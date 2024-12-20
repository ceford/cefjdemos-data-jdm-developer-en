<!-- Filename: J4.x:Setting_Up_Your_Local_Environment / Display title: Setting Up a Local Environment -->

## Quick Start Guide

For a test setup you will need a software stack that includes Apache, MySQL and PHP. The steps required are covered elsewhere in this manual. If in doubt use a software stack appropriate for your platform.

## Additional Tools

For testing Joomla pull requests and contributing to core Joomla code you will need additional tools, all easily installed, often with a one-line command:

1. **Composer** - for managing Joomla's PHP dependencies. Read the [Composer documentation](https://getcomposer.org/doc/00-intro.md) for help with installation.
2. **Node.js** - for compiling Joomla's JavaScript and SASS files. Read the the [Node.js documentation](https://nodejs.org/en/) for help with installation.
3. **Git** - for version management. Read the [git documentation](https://git-scm.com/) for help with installation.

## Steps to Setup a Local Test Site

1. Go to the [Joomla! repository on GitHub](https://github.com/joomla/joomla-cms).
2. Select the Joomla branch to work on. This selects the appropriate README instructions.
3. Scroll down the page to the **Steps to setup the local environment:** section of the README text.
4. Follow the steps listed there. You may change the name of the cloned joomla-cms folder so you can make as many clones as you like for different purposes.
5. Create a database and install Joomla but do not delete the installation folder at the end of the installation process.

## Updating Joomla

From time to time you may need to update a Joomla clone. This is a one-line command that is usually quick. However, it is usually necessary to rebuild the CSS and JavaScript files, which is a more complicated and longer process.

Linux and OSX users can set up the following bash alias by placing the following inside the *~/.bashrc file*:

```sh
    alias jclean="rm -rf administrator/templates/atum/css; \
    rm -rf templates/cassiopeia/css; \
    rm -rf administrator/templates/system/css; \
    rm -rf templates/system/css; \
    rm -rf media/; \
    rm -rf node_modules/; \
    rm -rf libraries/vendor/; \
    rm -f administrator/cache/autoload_psr4.php; \
    rm -rf installation/template/css"
    alias jinstall="jclean; composer install; npm ci"
```

This will delete all the compiled files and run a fresh install as one command by calling `jinstall` inside your Joomla install. You can also use the `jclean` command to swap back to a different Joomla branch.

**Note:** You may need to run `composer install` with the `--ignore-platform-reqs` option to ignore platform requirements specified in Composer. That is, if you do not have PHP's LDAP extension installed.

### Database changes

If a Joomla update includes database changes you may need to find the individual changes and run them manually or start again with a new clone.

## Node/npm Scripts

The installation of Joomla from a repository clone used two commands:

- **composer install** Install all the needed composer packages.
- **npm ci** Install all the needed npm packages.

Node.js comes with a package manager called NPM that has a `run` command. Some scripts are available to make the build process quicker if only CSS or JavaScript files have been changed.

### npm run build:css

This command compiles SASS files to CSS and also creates the minified files.

### npm run build:js

This command compiles and transpiles the JavaScript files to the correct format and creates minified files.

### npm run watch

This command is the same as the `build:js` command but will watch for changes and automatically build updated files in the media directory. SASS files are not included yet.

### npm run lint:js

This command performs a syntax check on all ES6 JavaScript files against the JavaScript code standard. For more information see the [Joomla coding standards manual](https://developer.joomla.org/coding-standards/introduction.html).

### npm run test

This comman will run a JavaScript testing suite.

## Possible Issues

When running composer install you can run into these errors

```sh
    Problem 1
        - Installation request for joomla/ldap 2.0.0-beta -> satisfiable by joomla/ldap[2.0.0-beta].
        - joomla/ldap 2.0.0-beta requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
    Problem 2
        - Installation request for symfony/ldap v5.1.5 -> satisfiable by symfony/ldap[v5.1.5].
        - symfony/ldap v5.1.5 requires ext-ldap * -> the requested PHP extension ldap is missing from your system.
```

The solution is to run the composer install with the `--ignore-platform-reqs` option to ignore platform requirements specified in Composer. That is, if you do not have PHP's LDAP extension installed.

```sh
    composer install --ignore-platform-reqs
```

If you receive a login error, delete the `administrator/cache/autoload_psr4.php` file.
