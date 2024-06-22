<!-- Filename: J4.x:Improved_Override_Management / Display title: Improved Override Management -->

<span id="main-portal-heading">GSoC 2018  
Improved Override Management  
Documentation</span> [<img
src="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/75px-Gsoc2016.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/113px-Gsoc2016.png 1.5x, https://docs.joomla.org/images/thumb/7/7d/Gsoc2016.png/150px-Gsoc2016.png 2x"
data-file-width="373" data-file-height="373" width="75" height="75"
alt="Gsoc2016.png" />](https://docs.joomla.org/GSOC_2018 "GSOC 2018")
Joomla!  4.x

## Introduction

This project adds an update checking feature to Joomla so that if a
template overrides core file is changed or updated, it notifies the user
that one of the core files of their template overrides is changed with
the update, to avoid security issues or functionality issue and they can
adjust their override before anyone can notice.

**Project repository link:**
<a href="https://github.com/joomla-projects/gsoc18_override_management"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla-projects/gsoc18_override_management</a>

## Getting a head-start with Improved Override Management

### Notes

If you are new to Joomla development, and don't know much about Template
Manager and Overrides, please read:

1.  [How to use the Template
    Manager](https://docs.joomla.org/J3.x:How_to_use_the_Template_Manager "Special:MyLanguage/J3.x:How to use the Template Manager")
2.  [Layout Overrides in
    Joomla](https://docs.joomla.org/Layout_Overrides_in_Joomla "Special:MyLanguage/Layout Overrides in Joomla")

Now, if you got familiar with how to use Template Manager and types of
Overrides in Joomla. Then, let's go through this project features.

- Supported Overrides
- Diff View
- Override - Quick Icon Notification Plugin
- Installer - Override Plugin
- Updated - Override History

## Types of overrides supported by this feature

It does not support Alternative Layout in which filename get renamed to
something else and js, css overrides. This feature supports these
override files listed in Create Override tab. Example:

<img
src="https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/800px-Selection_035-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/1200px-Selection_035-en.png 1.5x, https://docs.joomla.org/images/thumb/a/aa/Selection_035-en.png/1600px-Selection_035-en.png 2x"
data-file-width="1907" data-file-height="956" width="800" height="401"
alt="Selection 035-en.png" />

## Diff view between core and override files

This feature shows the difference between the core file and the override
file. When you open any override file to edit you can see two buttons or
switchers in the top right corner of the page if core file of that file
is exited.

Buttons like this:

<img src="https://docs.joomla.org/images/6/66/Selection_027-en.png"
decoding="async" data-file-width="421" data-file-height="197"
width="421" height="197" alt="Buttons" />

Here, you can control the hide and show view of diff view and core file
view. In the next image, you will see the location of the core file and
diff view in the template manager.

<img
src="https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/800px-Selection_028-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/1200px-Selection_028-en.png 1.5x, https://docs.joomla.org/images/thumb/f/f2/Selection_028-en.png/1600px-Selection_028-en.png 2x"
data-file-width="1904" data-file-height="952" width="800" height="400"
alt="Selection 028-en.png" />

You can not edit the core file.

### How diff view is working

When you click on any override file to edit it, it calls a function
method `getCoreFile` which receives the two parameters `path` of the
override file in the template. Example :
`/html/layouts/joomla/form/field/user.php` and the client path of the
file whether it belongs to `Site` or `Administrator`. Then, based on
these information it returns the path of the core file if it exists. For
showing the diff between the core and override files we used
<a href="https://github.com/kpdecker/jsdiff" class="external text"
target="_blank" rel="nofollow noreferrer noopener">jsdiff</a> library.

## Override - Quick Icon Notification Plugin

A quick icon notification plugin which shows the notification in cpanel
and it shows the total updated overrides from all templates. When
overrides are updated then the quick icon shows something like in the
example below:

<img
src="https://docs.joomla.org/images/thumb/5/51/Selection_020-en.png/800px-Selection_020-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/5/51/Selection_020-en.png 1.5x"
data-file-width="1080" data-file-height="309" width="800" height="229"
alt="Quick Icon" />

When you click on it, it will redirect you to Templates which contain
the list of your templates with their description. You'll see a new
column header **Overrides** showing the number of override updated which
belongs to the template. If nothing has been updated in the template
override it will show a Up to date badge.

<img
src="https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/800px-Selection_022-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/1200px-Selection_022-en.png 1.5x, https://docs.joomla.org/images/thumb/d/d6/Selection_022-en.png/1600px-Selection_022-en.png 2x"
data-file-width="1897" data-file-height="662" width="800" height="279"
alt="Templates" />

### How quick icon is working

It makes an AJAX call to the `TemplateController.php` which returns the
information and displays a notification when such overrides are updated.

**Warning**

Quick icon notification plugin only works or able to fetch data if
`installer/override` plugin is enabled. If `installer/override` is
disabled then you will see this error message in quick icon.

<img
src="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/800px-Selection_024-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/9/9d/Selection_024-en.png/1200px-Selection_024-en.png 1.5x, https://docs.joomla.org/images/9/9d/Selection_024-en.png 2x"
data-file-width="1300" data-file-height="333" width="800" height="205"
alt="Enable" />

If you click on the quick icon you will be redirected to the
`installer/override` plugin settings where you can edit the settings of
the plugin.

## Installer - Override Plugin

This plugin is the main part of this feature. It allows to find the
correct updated overrides during the extension install or update and
Joomla update.

<img
src="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/800px-Selection_025-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1200px-Selection_025-en.png 1.5x, https://docs.joomla.org/images/thumb/f/fe/Selection_025-en.png/1600px-Selection_025-en.png 2x"
data-file-width="1887" data-file-height="719" width="800" height="305"
alt="Installer Override" />

### How installer override plugin is working

This plugin works on 6 events:

- onExtensionBeforeUpdate
- onExtensionAfterUpdate
- onInstallerBeforeInstaller
- onInstallerAfterInstaller
- onJoomlaBeforeUpdate
- onJoomlaAfterUpdate

Which collect all overrides core file `md5_file()` hash before update
and after update then compares both values. Then, find the correct
changed or updated file. And, store information in
`#__templates_overrides` table.

## Updated - Override History

You can access this from the Updated Files tab in a template. This is a
list view that shows the list of updated overrides which do belongs to
that template.

<img
src="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/800px-Selection_029-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1200px-Selection_029-en.png 1.5x, https://docs.joomla.org/images/thumb/b/b9/Selection_029-en.png/1600px-Selection_029-en.png 2x"
data-file-width="1901" data-file-height="737" width="800" height="310"
alt="Selection 029-en.png" />

There are many options available to manage list. Where you can check the
status of override file history whether is checked or not, created date,
date of change and update action like: whether it belongs to (Joomla
Update, Extension Update or Extension Install).

**Note**

These information are only history so if you checked the updated
override changes then, you can delete the history, as not needed
any more.

**Watch the video tutorial to learn how to use this feature.**

<div class="video-container">
<p><iframe src="//www.youtube.com/embed/l0_jFjswObI" allowfullscreen="" width="425" height="355" frameborder="0"></iframe>
</p></div>
