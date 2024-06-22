<!-- Filename: Setting_up_Eclipse_PDT_2020_and_Git_for_Pulls / Display title: Setting up Eclipse PDT 2020 and Git for Pulls -->

## Introduction

I first started using Eclipse many years ago for a private Joomla
project and a private remote git repository not on Github. I read the
available tutorials and happily muddled along until this year when I
moved on to try to help with core testing and bug-fixing for Joomla! 4.
And then some pull requests. That is when I realized I had not
understood exactly what I was doing. Eventually I decided to start again
and document the process for other semi-experienced developers who are
new to Joomla, especially the parts I did not fully appreciate the first
time around. First on the list:

## File Structure

It is not obvious before you download and install Eclipse that there are
at least two places where data are stored apart from wherever the
Eclipse code is located.

### The Workspace

This is where Eclipse stores its own data for each project. It should
not be in the web tree! As I work mostly on a Mac laptop the default
location for me is **/Users/username/eclipse-workspace**. There is a
sub-folder for each project. So one could be
**/Users/username/eclipse-workspace/joomla-cms**. It contains one
folder: .metadata. Linux users will probably know to substitute home for
Users.

### The git local repository and test web site

This is where project code is stored. It could be in the local web tree
if you want to use it for testing. For me it is
**/Users/username/Sites/joomla-cms-4**. Linux users will probably know
to substitute public_html for Sites. Watch out for the extra steps
needed to make the local repository work as a Joomla site.

### A separate test web site

This is optional but requires the git repository to have a custom
build.xml file, say build-local.xml, covered in the Appendix.

## Required Software

To set up a working development and test web site you will first need to
install the following software:

- Apache
- MySQL or Maridb
- PHP
- phpMyAdmin -
  <a href="https://www.phpmyadmin.net/" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">https://www.phpmyadmin.net/</a>
- Composer -
  <a href="https://getcomposer.org/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://getcomposer.org/</a>
- Node.js -
  <a href="https://nodejs.org/en/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://nodejs.org/en/</a>
- Eclipse PDT -
  <a href="https://www.eclipse.org/pdt/" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">https://www.eclipse.org/pdt/</a>
- git (optional for command line git users) -
  <a href="https://git-scm.com/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://git-scm.com/</a>
- phing (optional if you want to do custom builds)
  <a href="https://www.phing.info/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://www.phing.info/</a>

The first three or four often come as a bundle for your platform, known
as a LAMP, WAMP or XAMP stack. Just use whatever your platform offers or
look here:
<a href="https://www.apachefriends.org/" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://www.apachefriends.org/</a>

Let us assume you have installed everything except Eclipse:

## Fork joomla-cms on Github

There is a workflow described in [My first pull request to Joomla! on
Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github")
that I cannot praise too highly. It shows exactly what you need to do:

<img
src="https://docs.joomla.org/images/thumb/a/a4/Github_work_flow_joomla.png/800px-Github_work_flow_joomla.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/a/a4/Github_work_flow_joomla.png 1.5x"
data-file-width="1101" data-file-height="798" width="800" height="580"
alt="Github work flow joomla.png" />

Steps

- Go to Github and obtain an account, free and quick:
  <a href="https://github.com/" class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/</a>
- In your github account, go to the joomla/joomla-cms repo: type
  joomla/joo... in the top left search box and select when options
  appear.
- In the joomla/joomla-cms repo click the fork button at the top right.
  That gives you a forked copy of all of the code for Joomla 3, Joomla
  4, ..., everything, in your own Github account.

Back to your workstation.

## Install Eclipse

I have two versions of Eclipse already installed, Oxygen from a few
years ago, and Cocoa from 2020. From here on I am installing a second
instance of Cocoa. Let's see what happens:

- Go to the Eclipse site and download the version for your platform:
  <a href="https://www.eclipse.org/pdt/" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">https://www.eclipse.org/pdt/</a>
- Follow the installation procedure and eventually start the the Eclipse
  application, for me Eclipse 2.app.
- Warning: “Eclipse 2.app” is an app downloaded from the internet. Are
  you sure you want to open it? **Open**
- Select a directory as workspace - Default is
  /Users/username/eclipse-workspace. **Browse** and create a
  joomla-cms-4 sub-folder.
- Launch: Eclipse is installed and Displays the Welcome page.
- Review IDE Configuration settings. Set items 1, 2, 5 and 6.
- Select Workbench icon at the top right.

(Instead of installing another version of Eclipse I could have opened a
new empty workspace.)

So far so good!

## Import fork into Eclipse

In your new local installation of Eclipse:

- Open Preferences and set Team / Git / Default repository folder to
  /Users/username/git (you may or may not use that location)
- Open File / Import / Git / Projects from Git (with smart import). Next
  ...
- Clone URI. Next ...
- Copy the URL bar from **your Github fork** and paste it into the URL
  field. You don't need to add Authentication credentials. Next ...
- Branches to import ... er ... Probably best to import everything. Next
  ...
- Directory. This is where you could choose a different location to keep
  the code. I browsed to /Users/username/public_html/joomla-cms-4,
  creating it if necessary.
- Initial branch - if you imported all branches. I am working on Joomla
  4 so I selected 4.0-dev. And I noted that my clone will be **origin**.
  Next ...
- Sip of coffee. Cloning will take a few minutes.
- Import source. The source should be the code location. Finish.
- Eclipse now shows the root of the code tree in the Project Explorer
  panel. Click to see more of the tree. Notice that this code will not
  work as a Joomla site yet. Among other things, there is no media
  folder.

## Add original joomla-cms repository to Eclipse

- Show the Git Repositoris window: select Window / Show View / Other
- Select Git / Git Repositories - the window appears at the bottom of
  the screen.
- Expand joomla-cms / remotes / origin - if you make changes to your
  code and push to origin this is where it goes.
- Right click on Remotes and select Create Remote...
- Set the Remote name to **joomla-origin** and select **Configure
  fetch**.
- In Configure Fetch select **Change**.
- In Select a URI paste in the url of the original joomla-cms
  repository:
  <a href="https://github.com/joomla/joomla-cms" class="external free"
  target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>
- Leave Credentials blank. Finish.
- in Configure Fetch: **Save and Fetch**.

## Create a working site

Your copy of the joomla-cms code needs more steps to make it usable as a
web site.

- Open a terminal and switch to the folder containing your cloned code.
- Run composer install:
  - Linux and OSX users can set up the following bash alias by placing
    the following inside the ~/.bash_profile or ~/.zsh file (\$ source
    ~/.bash_profile to effect immediately):
    alias jclean="rm -rf administrator/templates/atum/css; rm -rf templates/cassiopeia/css; rm -rf media/; rm -rf node_modules/; rm -rf libraries/vendor/;rm -f administrator/cache/autoload_psr4.php;rm -rf installation/template/css"
    alias jinstall="jclean; composer install --ignore-platform-reqs; npm ci"
- composer is not in my path so I substituted php
  ~/composer/composer.phar
- jinstall
- that fetches all of the Joomla PHP dependencies, Javascript
  dependencies, compiles all ES6 Javascript and puts the files in their
  appropriate locations.
- sip of coffee while the dependencies are downloaded and media files
  created.
- In Eclipse right click on the project root and select Refresh. You
  will see your code now has a media folder.
- If you are interested, use a file manager to see that the root also
  contains a file name .gitignore and the media folder is mentioned in
  it. This means it won't be committed to your local or remote forked
  git repositories.

You are now ready for a Joomla installation:

- Create a database using phpMyAdmin (or the mysql command line if you
  prefer).
- I created a database named joomla-cms-4 with utf8mb4-unicode-ci
  collation.
- Create a new user: mine is jcms4 with a generated random password
  (GAOC26r77bBLkkdA). You need to note down the password for use during
  installation. It ends up in plain text in the configuration file.
- Grant All Priveleges on the database to this user - the default.
- In your favourite browser go to the root of the new site:
  localhost/joomla-cms-4/

### On my Mac running macOS Catalina

- Environment Setup Incomplete: More details. Bah - just fix it and
  check the url bar contains the url you typed in - mine had somehow
  appended some extra characters.
- Otherwise it goes through to a working site - do not delete the
  installation folder.

### On my Linux workstation running Linux Mint 20

Oops! Something went wrong!

The snag is that I have my Joomla code in my personal file space for
read/write access by Eclipse. The web server also needs write access to
write configuration, cache and log files but it is running as a user and
group with low privileges. As I am on a private home network I edited
/etc/apache2/apache2.conf to comment out User \${APACHE_RUN_USER} and
Group \${APACHE_RUN_GROUP} and add User myusername and Group
mygroupname. Restart apache, then...

- It goes through to a working site - do not delete the installation
  folder.

## Code Revision

In brief:

- Fetch from joomla-origin to make sure my local clone is up to date.
- Team / Merge / Select branch to merge - care - I selected
  joomla-origin/4.0-dev
- Push to origin to make sure my remote fork is up to date.
- Create a Branch for some code changes I want to make.
- Do the code changes
- If the changes are to css or js source files (those in sass or es6) go
  to a terminal window and run jinstall again.
- TEST your local installation to see if there any problems.
- Commit the code changes
- Push to origin to update my remote fork with my changes.
- Go to your own account on Github and select the branch you created
  with the updated code. Something scary: it says **This branch is 11084
  commits ahead, 134 commits behind joomla:staging.** Did I do something
  wrong? Apparently not!
- Select the Pull Request button. Make sure that you select the correct
  joomla branch to merge into. For me this is 4.0-dev. And make sure you
  have selected your branch with the altered code. Go for it!
- The pull request has to be tested and approved and may take days,
  weeks or months. And the change may be rejected!

## Meanwhile

Back on your workstation:

- Switch back to your original branch, for me: Team / Switch To /
  4.0-dev
- Rebuild: for me Project / Build Project
- See the files previously changed being copied to your working site
  again.
- TEST: your working site is back to the way it was before your code
  changes.

You are now ready to make another branch for another set of code
changes.

## If Disaster Strikes

At one stage my local clone somehow became corrupted and I had no idea
how to fix it. So I deleted my local clone and all its associated files,
emptied the database, and then went back to the **Import fork into
Eclipse** stage above. That put my local clone in sync with my remote
fork, including any branches I created for pull requests. The new
install worked without a hitch and I was happy!

## Other Resources

- [Configuring Eclipse for Joomla
  development](https://docs.joomla.org/Configuring_Eclipse_for_joomla_development "Special:MyLanguage/Configuring Eclipse for joomla development")
  (2012-2013) But get the lateste Eclipse PDT version!
- [My first pull request to Joomla! on
  Github](https://docs.joomla.org/My_first_pull_request_to_Joomla!_on_Github "Special:MyLanguage/My first pull request to Joomla! on Github")
  (2011-2014) Good overview if a little out of date.
- [Working with git and
  github](https://docs.joomla.org/Working_with_git_and_github "Special:MyLanguage/Working with git and github")
  (2011-2015)
- [Setting Up Your Local
  Environment](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment "Special:MyLanguage/J4.x:Setting Up Your Local Environment")
  (2018-2020)
- [Configuring Eclipse and
  Xdebug](https://docs.joomla.org/Configuring_Eclipse_and_Xdebug "Special:MyLanguage/Configuring Eclipse and Xdebug") (2013)
  All about debugging.
- [Working with Git and
  Eclipse](https://docs.joomla.org/Working_with_Git_and_Eclipse "Special:MyLanguage/Working with Git and Eclipse") (2014)
  Method for old editions of Eclipse.
- [Running Automated Tests from
  Eclipse](https://docs.joomla.org/Running_Automated_Tests_from_Eclipse "Special:MyLanguage/Running Automated Tests from Eclipse") (2020)
  For advanced users?
- [Configuring Xdebug for PHP
  development/Linux](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development/Linux "Special:MyLanguage/Configuring Xdebug for PHP development/Linux") (2016)
  Install and configure.
- [Configuring Eclipse IDE for PHP
  development/Linux](https://docs.joomla.org/Configuring_Eclipse_IDE_for_PHP_development/Linux "Special:MyLanguage/Configuring Eclipse IDE for PHP development/Linux") (2019)
  Install and configure for Linux.
- [Configuring Xdebug for PHP
  development](https://docs.joomla.org/Configuring_Xdebug_for_PHP_development "Special:MyLanguage/Configuring Xdebug for PHP development") (2014)
  Steps for Linux, Windows and Mac OS X.
- [Webinar: Using Eclipse for Joomla!
  Development](https://docs.joomla.org/Webinar:_Using_Eclipse_for_Joomla!_Development "Special:MyLanguage/Webinar: Using Eclipse for Joomla! Development") (2014)
  This 45-minute video webinar, recorded on April 30, 2009, provides an
  overview of Eclipse features for Joomla! development.
- [Setting up your workstation for Joomla
  development](https://docs.joomla.org/Setting_up_your_workstation_for_Joomla_development "Special:MyLanguage/Setting up your workstation for Joomla development") (2020)
  Brief overview of software required and alternative IDEs.

## Appendix

At one stage I had my local git repo outside my web root and needed to
copy any changes I made to a separately installed local site. That
needed a custom build file, shown here for reference:

### Create a build-local.xml file

The Eclipse clone contains a build.xml file but it is used to test and
build a downloadable zip file for a new installation. What I want to do
is copy any changes I make to my cloned code to my test site on my
laptop. Note that I only want to make changes to PHP code and not to
Javascript or CSS. To do that I created a separate file named
build-local.xml in the root of the project:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="joomla-cms" basedir="." default="main">
    <property file=".project" />

	<property name="joomladir" value="/Users/username/public_html/joomla-cms"  override="true" />
    
	<property name="srcdir" value="${project.basedir}" override="true" />

    <!-- Fileset for all files -->
    <fileset dir="${srcdir}" id="allfiles">
        <include name="administrator/**" />
        <include name="api/**" />
        <include name="cli/**" />
        <include name="components/**" />
        <include name="images/**" />
        <include name="includes/**" />
        <include name="language/**" />
        <include name="layouts/**" />
        <include name="libraries/**" />
        <include name="modules/**" />
        <include name="plugins/**" />
        <include name="templates/**" />
        <include name="index.php" />

        <exclude name="**/.*" />
    </fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">
        <copy todir="${joomladir}">
        	<fileset refid="allfiles" />
        </copy>
    </target>
</project>
```     

### Add a build tool

- Right click on the project root and select Properties.
- Select Builders / New / Program / OK.
- Name: Local build with phing
- Location: wherever phing is installed. For me it is
  /usr/local/php5/bin/phing even though I am using PHP 7.4.
- Working Directory: Browse Workspace and select joomla-cms - comes out
  as \${workspace_loc:/joomla-cms}
- Arguments: -f build-local.xml
- Apply / OK
- In Builders: Apply and Close
- Project / Build Project
- See what happens:

```bash
    Buildfile: /Users/username/git/joomla-cms/build-local.xml
     [property] Loading /Users/username/git/joomla-cms/.project

    joomla-cms > main:


    BUILD FINISHED

    Total time: 0.2121 seconds
```
