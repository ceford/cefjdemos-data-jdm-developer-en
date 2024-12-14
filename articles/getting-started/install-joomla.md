<!-- Filename: J4.x:Developer:_Required_Software / Display title: Install Joomla -->

## Summary

This is just a short summary of the steps involved:

- **Download** the latest full version from the [Joomla Downloads](https://downloads.joomla.org/) page.
- **Save** the download file in your Document root or a subfolder of the root.
- **Uncompress** the download file in place. Some operating systems will create a folder with the same name as the download zip file, minus the zip. That is good - you can just rename the folder with a short name and move it if necessary. Other operating systems will unpack the files and folders in place. That is bad if you have put the download in your root folder where there are existing folders with other sites. You will have to create a short folder name and move all the newly extracted files and folders into it. The objective is to end up with a folder that you can access with your web browser via localhost/j4test.
- **Install** point your browser at localhost/j4test and fill out the Joomla installation forms.

## Configuration

Some suggestions:

- **Global Configuration / Site / Cookie / Cookie Path** set to the folder containing you joomla installation (/j4test).
- **Global Configuration / System / Debug / Debug System** set to Yes.
- **Global Configuration / System / Session Lifetime** set to 60 - otherwise get logged out whilst thinking.
- **Global Configuration / Server / Server / Error Reporting** set to Maximum.
- **Plugin System - Debug / Plugin / Refresh Assets** set to No unless you are debugging css or javascript.

That is it. If Joomla is working you are ready to use it for extension development.

## More Sites

You can install as many sites as you like on a single computer. Depending on how you have set up your server that may be via different subfolders accessed via localhost/test1, localhost/test2 and so on or by virtual hosts accessed by test1.localhost, test2.localhost and so on. Either way, you can have a new database and a new clean Joomla site in a few minutes. Choose meaningful short names! *test1* and *test2* will soon become confusing.

## Installation for Testing

There is a different procedure if you want to help with Joomla testing. In this case you should install **git** and follow the instructions in the [GitHub Repository](https://github.com/joomla/joomla-cms).

- in GitHub, select the Joomla branch to work on. That changes the instructions seen further down the page.
- In your laptop or desktop, follow the instructions from *Clone the repository:* onwards.
- Do not delete the Installation directory at the end of Joomla installation!
- Rename the clone from joomla-cms to something else so you can do it all again later.
- Install the Joomla Patchtester.
