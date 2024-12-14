<!-- Filename: J4.x:Developer:_File_Structure / Display title: Example File Structure -->

## Introduction

As an individual starting out with some Joomla extension development work on a personal computer, laptop or desktop, you need to set up a suitable file structure for your extension code. The location of your Joomla site is predefined by your operating system and Apache installation. The location of the code you create is up to you.

## Test Web Site Location

In this example, Joomla test sites are located in a sub-folders of the document root. This allows creation of many web sites for all sorts of different projects without the need to create virtual hosts. Some test sites may not related to Joomla at all. The site root for different platforms:

- Mac: /Users/username/Sites
- Linux: /home/username/public_html
- Windows: ...

This is a screenshot of part of a Sites folder list showing a selection of many test sites:

![multiple sites on mac](../../../en/images/getting-started/developer-file-structure-mac-sites.png)

Each is accessed by its subfolder name. Examples:

- `http://localhost/j4test`
- `http://localhost/j520`

There are circumstances in which you may prefer to create separate virtual sites. That is not covered here.

## Test Web Site File Structure

If you have not done so already, you will need to become familiar with the structure of a Joomla web site. The following illustration shows a typical Joomla file and folder tree, with the Administrator folder expanded to show its contents.

![joomla file structure with administrator expanded](../../../en/images/getting-started/developer-file-structure-mac-joomla.png)

This is where the working code will be installed. The source code is somewhere else.

## Developer Extension Code Location

The location of your extension code is a personal choice. I like to keep my extension code in a file tree suitable for creation of an installable zip file. The base of my tree is /Users/username/git because I can spell git and I use git for version control. You do not have to do that - git will be covered in a separate tutorial. My git parent folder contains many subfolders that each may use separate git folders for version control. This is a screenshot showing a partial list of projects:

![joomla file structure project folders](../../../en/images/getting-started/developer-file-structure-mac-project-folders.png)

Notice that some of the folder names start with `j4xdemos`, which I adopted to use as the first part of the namespace used for my projects created for Joomla 4 tutorial purposes. It is not necessary as part of the folder name but it is something to think about: the first part of your namespace needs to be something unique to you or your organisation. I have since adopted `cefjdemos` as my namespace prefix as it is more personal and not Joomla version specific.

### Project Folder Structure

Taking the j4xdemos-com-mywalks as an example, all of the code that will go into the extension is inside the com_mywalks folder. When this is compressed, the com_mywalks.zip file created is used for installation in Joomla. None of the other files in the root are included in the zip file but may be included in the GitHub Repository. For example, README.md is a Markdown format text file containing a brief description of the extension. The folder name j4xdemos-com-mywalks is not significant. It is not used anywhere other than to sort the folders into alphabetical order.

In the following illustration the j4xdemos-com-mywalks folder has been opened in VSCodium to show the project code structure. The mywalks.xml file is a manifest file that tells Joomla what to install where. The admin and site folders contain code that will go into administrator/components/com_mywalks and components/com_mywalks.

![Project folder open in vscodium](../../../en/images/getting-started/developer-file-structure-mac-vscodium.png)

It should be obvious that even a small component needs quite a lot of folders and files. There are extension boilerplate building tools available to quickly create a skeleton component. They are covered elsewhere. ToDo

Ready to create some code?
