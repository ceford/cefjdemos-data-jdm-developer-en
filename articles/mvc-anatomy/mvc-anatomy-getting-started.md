<!-- Filename: J4.x:MVC_Anatomy:_Getting_Started / Display title: MVC Anatomy: Getting Started -->

## Introduction

Joomla components follow the Model, View, Controller approach, or MVC
for short. The Model is supposed to handle loading and storing of data.
The View is supposed to handle display of data. And the Controller is
supposed to handle program flow, the interaction between the component
Model and View code.

If you want to build your own component there are two approaches to
learning you might adopt:

- **Build it step by step:** by following a simple tutorial that
  describes each stage.
- **Dissect it file by file:** by taking a working component apart to
  learn how each part works.

This tutorial uses the latter approach. The component used for the
tutorial is named com_countrybase. It is used to manage and display some
basic data on the countries of the world. It is actually useful in its
own right and could be used as a basis for building something more
sophisticated.

## Outline Objectives

Whatever your project you should start with an outline of your
objectives and perhaps ask yourself whether those objectives can be
attained with Joomla core components or an available extension. For
com_countrybase a table of country data is needed, together with input
and output pages. And perhaps a module to display a daily monetary
exchange rate. And even a plugin called by a cli command to update
currency data at regular intervals. This tutorial just covers the
component.

The component needs a database table to store country data and a table
to store currency data. Each will need a list view and an edit view.
This is not something that can be accomplished with Joomla core
components so it is clearly a case for a custom component.

## Starter Tables

There is a lot of data about countries available from all sorts of
sources in all sorts of formats. As there are at least 250 countries in
the world, entering data for each country one by one using a data entry
form would be quite tedious. So, I prepared starter tables by collecting
data from several sources, combining them in a spreadsheet and then
exporting sql statements to create the tables.

The data are based on ISO 3166 Country Codes but some very well-known
countries are not included, for example England, Scotland and Wales. You
do not need to worry about this. The tables are provided with the
com_countrybase component. The table creation code in the component is
described later.

The files for the com_countrybase component are available from Github:

<a
href="https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/ceford/j4xdemos-com-countrybase/archive/refs/heads/master.zip</a>

Download and install the component to see it working in the
Administrator menu. Create a menu item if you wish to see it working in
your Site template. Also, unpack the zip file in your project file
space, not in your test web-site tree. You can then examine the
component file structure and file content with your favourite IDE or
text edit tool.

## Screenshot

This Administrator list of Countries has five items to minimise image
size. Joomla normally displays 20 items.

<img
src="https://docs.joomla.org/images/thumb/2/29/J4x-mvc-anatomy-getting-started-countries-list-en.png/800px-J4x-mvc-anatomy-getting-started-countries-list-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/2/29/J4x-mvc-anatomy-getting-started-countries-list-en.png/1200px-J4x-mvc-anatomy-getting-started-countries-list-en.png 1.5x, https://docs.joomla.org/images/2/29/J4x-mvc-anatomy-getting-started-countries-list-en.png 2x"
data-file-width="1440" data-file-height="665" width="800" height="369"
alt="Countries list" />

## Boilerplate Component

To help you get started with your own component there is a boilerplate
component available from Github:

<a
href="https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/ceford/j4xdemos-com-bpsrc/archive/refs/heads/master.zip</a>

Download and unpack this in your project file space, not in your test
web-site tree. After download, make all the changes indicated in the
README and you are ready to go.

## Related Tutorials

### MVC Anatomy

- [Getting
  Started](https://docs.joomla.org/J4.x:MVC_Anatomy:_Getting_Started "Special:MyLanguage/J4.x:MVC Anatomy: Getting Started")
- [File
  Structure](https://docs.joomla.org/J4.x:MVC_Anatomy:_File_Structure "Special:MyLanguage/J4.x:MVC Anatomy: File Structure")
- [Manifest
  File](https://docs.joomla.org/J4.x:MVC_Anatomy:_Manifest_File "Special:MyLanguage/J4.x:MVC Anatomy: Manifest File")
- [Site
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Site_Files "Special:MyLanguage/J4.x:MVC Anatomy: Site Files")
- [Administrator Startup
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Administrator_Startup_Files "Special:MyLanguage/J4.x:MVC Anatomy: Administrator Startup Files")
- [Administrator Edit
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Administrator_Edit_Files "Special:MyLanguage/J4.x:MVC Anatomy: Administrator Edit Files")
