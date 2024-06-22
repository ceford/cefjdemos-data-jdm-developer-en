<!-- Filename: Working_with_git_and_github / Display title: Working with git and github -->

## Introduction

This document will provide information about contributing to the Joomla!
CMS with the help of Git and GitHub. If you would like to make a simple change
(one file only), it is easier to use this documentation: [Using the
Github UI to Make Pull
Requests](https://docs.joomla.org/Using_the_Github_UI_to_Make_Pull_Requests "Special:MyLanguage/Using the Github UI to Make Pull Requests")
If you want to add more complex changes or you're just interested in
this, keep reading!

## What are Git and GitHub?

Git is a distributed version control system. It is a system that records
changes in files and keeps these changes in a history file. You can
always look back to an earlier version of your code and restore changes
if you like. Because of the history archive, Git is very useful when you
work with many people together on the same project.

Here is how GitHub can be used.
<a href="https://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a> is a website that helps
manage Git Projects in a visual way. As an owner of a project you can
change the code and compare different versions. As a user of the project
you can contribute by making a Pull Requests. A pull request is a
request to the owner to pull some code into the project. You're offering
a piece of code (perhaps a solution for a bug) and asking if the Project
owner would like to use it. If the owner likes it, he can merge (add) it
to his project.

Joomla! uses GitHub and Git to maintain its code. Anyone can
contribute to Joomla! software. The URL to the Joomla! CMS Project on
GitHub is:
<a href="https://github.com/joomla/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms</a>

## Sign up on GitHub and install Git

First, you will need to register at
<a href="http://www.github.com" class="external text" target="_blank"
rel="nofollow noreferrer noopener">GitHub</a>. It's free and easy to do.
Just follow the provided steps.

Once you are signed up, you need to install Git on the computer you use
for development (workstation or laptop). The latest version of Git
can be found on
<a href="http://git-scm.com" class="external free" target="_blank"
rel="nofollow noreferrer noopener">http://git-scm.com</a>. Download and
open the installer. Git is a CLI (Command Line Interface) program. At
the start this can be confusing and a bit scary but this document will
lead you through the process.

If you're not an advanced user, just run the installer and press the
"next" buttons until the program is installed. Git won't damage your
system. Later you can remove it just like any other program.

With Git installed, open a Terminal application. Start by setting 
your Git name and email address. Git will use this information when
you contribute to a project:

```bash
    git config --global user.name "Your name"
    git config --global user.email youremail@mail.com
```

In the above commands, and all other commands given in this
documentation, each line is a new command. So you type the fist line,
press enter and then type the second line and press enter.

Yoy are now ready to use Git and go further with setting up our test
installation.

## Setting up a test installation

For your test installation you need a Web server program so that you can
install and run Joomla! on your computer. There are a lot of programs
that can do that such as
<a href="https://www.mamp.info/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">MAMP</a> and
<a href="https://www.apachefriends.org/" class="external text"
target="_blank" rel="nofollow noreferrer noopener">XAMPP</a>. Download
and install one of them.

After the installation of such a program (MAMP is used in this
documentation), you need to install the latest version of Joomla!. 
This is not the last stable release.
The last version of Joomla! is the staging branch on GitHub. 

## Fork and Clone Joomla!

On GitHub you can find projects in so called Repositories. Inside a
project you might find several versions. Such a version is called a
Branch. Joomla! has the following branches:

- **4.2-dev** Files for development of the current version.
- **4.3-dev** Files for development of the next minor version.
- **4.4-dev** Files for development of the next-but-one minor version.
- **5.0-dev** Files for development of the next major version.

On your test computer you are going to use the **4.2-dev** branch. 
However, you cannot modify this branch because you are not its owner.
You need to make a copy of it. On GitHub this is called a Fork. You 
are the owner of that copy so you can modify it. After modifying your 
fork you can make a Pull Request for the changes you've made.
More about that later. You can Fork a branch by pressing the Fork button
on the
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">Joomla! CMS Github
Repository</a>. This button is located at the right top of the page.

<img src="https://docs.joomla.org/images/6/6c/Github-fork-button.png"
decoding="async" data-file-width="1002" data-file-height="222"
width="1002" height="222" alt="Github-fork-button.png" />

After forking, you need to install Joomla! on your local computer.
Go to the folder were you can place files used by your Web server. Many
programs use a folder called `htdocs`. Some use `www` and some use
other folders entirely. It all depends on whether you are using Windows,
Mac or Linux. Eventually, your web root will contain different folders
for different web sites. Once you are inside your web root folder.
Either, in an open Terminal window use the cd command to change the 
current directory to the web root. Or, in you file explorer GUI, find 
root web root folder, press the right mouse button and click on: 
"Git Bash Here" or "Open Terminal" or something similar. 

In the Terminal, with the site root folder set as the current folder,
type the following command to download the files from your Fork of the
Joomla! CMS to your computer. Please replace *username* with the
username you are using on GitHub.

```bash
    git clone https://github.com/username/joomla-cms.git
```
The clone process may take quite a while. When complete your web root 
will contain a folder named joomla-cms. You need to make that folder
the current folder to run git commands for that folder:

```bash
    cd joomla-cms
```

Please remember that for other commands in this documentation.

## Install Joomla!

Your downloaded clone of your forked repository needs further action
before it is ready for use. One of the downloaded files is named README.md.
Open it with a text editor and follow the instructions on 
**How to get a working installation from the source**.

When ready, open your browser and go to the installation on your
localhost. Usually the URL is something like:
<a href="http://localhost/joomla-cms" class="external free"
target="_blank"
rel="nofollow noreferrer noopener"><code>http://localhost/joomla-cms</code></a>.
You will now see the default Joomla! installation process.

## Make Code Changes

All changes you make to Joomla code on your local site will be registered 
and monitored by Git. At any moment you can type the
command `git status` to see which files are modified or untracked.
Untracked means that the file at that location is new for Git.

At this stage it is probably best to use an Integrated Development
Environment (IDE) to work on Joomla Code. Visual Studio Code (VSCode)
is highly recommended. It is free and works on all platforms. Using
VSCode you can make changes, **Commit** them to your local Git 
clone and **Push** them to your remote GitHub fork.

## Push Changes to the Fork

Uploading changes is called `push` in Git terms. Before you can do that,
you have to do something very important. You must create a new branch for
your changes. (A branch is a version of your project, remember?) If you
don't do that and make your change directly to the current branch, the
first time there won't be a problem. But if you make changes for a
second time, and the changes you made the first time are not merged yet,
all these changes will be registered too as new changes.

You can set up VSCode to accomplish all of the following commands
with a few clicks. But if you want to use git commands from the
terminal command line:

So the first command you are going to run will create a new branch. 
Replace name-new-branch with the name of the new branch. This name 
must be short, and can only contain lower case letters and numbers. 
Do **NOT** use spaces. Instead of spaces, use - (minus).

```bash
    git checkout -b name-new-branch
```

The next command tells git that all changes are ready to commit.

```bash
    git add --all
```

The following command adds your change to the branch. Please replace the
message with a short description of your changes. This description will
be the title of the pull request you are going to make.

```bash
    git commit -m "description"
```

The final command will push (upload) the changes to your fork. Please
replace name-new-branch with the name of the branch you made a few steps
above.

```bash
    git push origin name-new-branch
```

## Compare Forks and Make a Pull Request

After pushing your change to GitHub, go to your fork of the Joomla! CMS
on the GitHub site. Select your branch and make a pull request.

When finished, in your local clone, switch back to the original branch:

```bash
git checkout 4.2-dev
```

You can now make new changes without affecting the the changes in your
previous branch.

## Keeping up to dat

Because the current version of Joomla! can change every day, it is 
important to keep your fork up-to-date. You can do that by adding 
the original Joomla repository to your forked project:

```bash
    git remote add upstream https://github.com/joomla/joomla-cms.git
```

Then update your local clone with the following command:

```bash
    git pull upstream 4.2-dev
```

And update your remote fork:

```bash
    git push
```
