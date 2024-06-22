<!-- Filename: J4.x:Developing_an_MVC_Component/Language_Management / Display title: Developing an MVC Component/Language Management -->

Joomla!  4.x \>Tutorial Adding Language Management

**Articles in this Series**

1.  [Introduction](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Introduction "Special:MyLanguage/J4.x:Developing an MVC Component/Introduction")
2.  [Developing a Basic
    Component](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Developing_a_Basic_Component "Special:MyLanguage/J4.x:Developing an MVC Component/Developing a Basic Component")
3.  [Adding a View to the Site
    Part](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_View_to_the_Site_Part "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a View to the Site Part")
4.  [Adding a Menu Type to the Site
    Part](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Menu_Type_to_the_Site_Part "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a Menu Type to the Site Part")
5.  [Language
    Management](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Language_Management "Special:MyLanguage/J4.x:Developing an MVC Component/Language Management")
6.  [Adding a Model to the Site
    Part](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Model_to_the_Site_Part "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a Model to the Site Part")
7.  [Adding a Request Variable in the Menu
    Link](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Request_Variable_in_the_Menu_Link "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a Request Variable in the Menu Link")
8.  [Setting Up the
    Database](https://docs.joomla.org/J4.x:Developing_an_MVC_Componenthttps://docs.joomla.org/J4.x:Developing%20an%20MVC%20Component/Setting%20up%20the%20Database)

This article is part of the tutorial ["Developing an MVC Component for
Joomla
4.x"](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Introduction "J4.x:Developing an MVC Component/Introduction").
It is intended to be a follow-along programming tutorial, so if you have
not read the previous parts of the tutorial you are encouraged to do so.

## Basic Language Management

In this article we will add management for language strings to our new
component. You may have noticed in the last article in this tutorial
series that when we added a menu link to the component, we hard-coded
English words into the link:

```xml
<?xml version="1.0" encoding="utf-8"?>
<metadata>
    <layout title="Hello World!">
        <message><![CDATA[My first Joomla! page]]></message>
    </layout>
</metadata>
```

This works as an initial test, but hard-coding a single language into
the system is a bad idea. Before we get too far into building pages and
forms, we should add multi-language support to the component.

Even if you do not personally have the resources to perform translations
yourself, we ***highly recommend*** that you are diligent about
correctly managing your language strings inside your component. Open
source components can be translated by members of the community, and end
users can create their own if they so wish.

Let's create the language files and update the manifest to reference
them:

|     |                                                                                                                         |                                                     |
|-----|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| 1   | Create: [`admin/language/en-GB/en-GB.com_helloworld.ini`](#admin.2Flanguage.2Fen-GB.2Fen-GB.com_helloworld.ini)         | Component language strings for the admin panel part |
| 2   | Create: [`admin/language/en-GB/en-GB.com_helloworld.sys.ini`](#admin.2Flanguage.2Fen-GB.2Fen-GB.com_helloworld.sys.ini) | System language strings for the component           |
| 3   | Create: [`site/language/en-GB/en-GB.com_helloworld.ini`](#site.2Flanguage.2Fen-GB.2Fen-GB.com_helloworld.ini)           | Component language strings for the public site part |
| 4   | Update: [`helloworld.xml`](#helloworld.xml)                                                                             | Need to add the language files to the manifest      |
| 5   | Update: [`admin/tmpl/hello/default.php`](#admin.2Ftmpl.2Fhello.2Fdefault.php)                                           | Swap hard-coded language strings for language codes |
| 6   | Update: [`site/tmpl/hello/default.php`](#site.2Ftmpl.2Fhello.2Fdefault.php)                                             | Swap hard-coded language strings for language codes |
| 7   | Update: [`site/tmpl/hello/default.xml`](#site.2Ftmpl.2Fhello.2Fdefault.xml)                                             | Swap hard-coded language strings for language codes |

## File Details

**admin/language/en-GB/en-GB.com_helloworld.ini**

This file contains the language strings used on the component's pages
inside the admin panel of Joomla!

```ini
    ; Hello World Admin Strings
    ; Copyright (C) 2020 John Smith. All rights reserved.

    COM_HELLOWORLD_MSG_HELLO_WORLD="Hello World!"
```

**admin/language/en-GB/en-GB.com_helloworld.sys.ini**

This file contains the language strings used in Joomla! systems outside
the component's own pages. For example, our menu item's strings belong
to this component, but are used by the Joomla! menu system rather than
on our own pages, so they belong in this file.

```ini
    ; Hello World Sys.ini
    ; Copyright (C) 2020 John Smith. All rights reserved.

    COM_HELLOWORLD_MENU_HELLO_WORLD_TITLE="Hello World!"
    COM_HELLOWORLD_MENU_HELLO_WORLD_DESC="My first Joomla! page"
```

**site/language/en-GB/en-GB.com_helloworld.ini**

This file contains the language strings used on the component's "public"
pages (pages in the site part). There's a few things to note that are
common to all the language files you'll create. Firstly, language
strings are stored in the common INI format, and the name of each
property begins with the component's system name: `com_helloworld`. It
is convention that all language properties are written in upper-case.

You'll also notice that both the folder and the language file itself
contain the locale code for these strings, "en-GB". For each locale you
wish to support, you will need a new set of files with the appropriate
strings .

```ini
    ; Hello World Public Site Strings
    ; Copyright (C) 2020 John Smith. All rights reserved.

    COM_HELLOWORLD_MSG_HELLO_WORLD="Hello World!"
```

**helloworld.xml**

As before, we need to add the new `language` folders to the manifest, so
that Joomla! knows to copy them into place. Additionally, we need to
tell Joomla! that we have language files, where to find them and what
locales they support. This is done with the addition of two XML blocks,
one for each part of the component.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" version="4.0" method="upgrade">

    <name>Hello World</name>
    <!-- The following elements are optional and free of formatting constraints -->
    <creationDate>December 2020</creationDate>
    <!-- Dummy author, feel free to replace anywhere you see it-->
    <author>John Smith</author>
    <authorUrl>https://smith.ca</authorUrl>
    <copyright>John Smith</copyright>
    <license>GPL v3</license>
    <!--  The version string is recorded in the components table -->
    <version>0.0.4</version>
    <!-- The description is optional and defaults to the name -->
    <description>
        A hello world component!
    </description>

    <!-- This is the PHP namespace under which the extension's
    code is organised. It should follow this format:
    
    Vendor\Component\ComponentName

    "Vendor" can be your company or your own name
    
    The "ComponentName" section MUST match the name used 
    everywhere else for your component. Whatever the name of 
    this XML file is, the namespace must match (ignoring CamelCase). 
    -->
    <namespace path="src/">JohnSmith\Component\HelloWorld</namespace>

    <files folder="site/">
        <folder>language</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>

    <languages>
        <language tag="en-GB">site/language/en-GB/en-GB.com_helloworld.ini</language>
    </languages>

    <administration>
        <!-- The link that will appear in the Admin panel's "Components" menu -->
        <menu link="index.php?option=com_helloworld">Hello World</menu>
        <!-- List of files and folders to copy, and where to copy them -->
        <files folder="admin/">
            <folder>language</folder>
            <folder>services</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>

        <languages>
            <language tag="en-GB">admin/language/en-GB/en-GB.com_helloworld.ini</language>
            <language tag="en-GB">admin/language/en-GB/en-GB.com_helloworld.sys.ini</language>
        </languages>
    </administration>

</extension>
```

**admin/tmpl/hello/default.php**

Replace the hard-coded English strings with the new language properties.
We use the `Joomla\CMS\Language\Text` class to output our language
strings into the page. If the end user uses another language that is
supported in our component, this class will automatically fetch the
correct strings for that language and output those instead.

```php
<?php

use Joomla\CMS\Language\Text;

/**
 * @package     Joomla.Administrator
 * @subpackage  com_helloworld
 *
 * @copyright   Copyright (C) 2020 John Smith. All rights reserved.
 * @license     GNU General Public License version 3; see LICENSE
 */

 // No direct access to this file
defined('_JEXEC') or die('Restricted Access');
?>
<h2><?= Text::_('COM_HELLOWORLD_MSG_HELLO_WORLD') ?></h2>
```

**site/tmpl/hello/default.php**

Our page templates are currently identical, so the replacements required
are also identical. Remove the hard-coded English and replace it with
the correct language property.

```php
<?php

use Joomla\CMS\Language\Text;

/**
 * @package     Joomla.Administrator
 * @subpackage  com_helloworld
 *
 * @copyright   Copyright (C) 2020 John Smith. All rights reserved.
 * @license     GNU General Public License version 3; see LICENSE
 */

 // No direct access to this file
defined('_JEXEC') or die('Restricted Access');
?>
<h2><?= Text::_('COM_HELLOWORLD_MSG_HELLO_WORLD') ?></h2>
```

**site/tmpl/hello/default.xml**

Just like the PHP template files, we need to replace the hard-coded
English in this file with the new language properties. Unlike the PHP
files, however, we don't need any additional code to do so - simply
replace the English strings with the language property names and Joomla!
is clever enough to match them up for us.

Remember: because these strings are used by Joomla! in its own systems
(outside of our component's pages), they live in the special
[`admin/language/en-GB/en-GB.com_helloworld.sys.ini`](#admin.2Flanguage.2Fen-GB.2Fen-GB.com_helloworld.sys.ini)
file.

```xml
<?xml version="1.0" encoding="utf-8"?>
<metadata>
    <layout title="COM_HELLOWORLD_MENU_HELLO_WORLD_TITLE">
        <message><![CDATA[COM_HELLOWORLD_MENU_HELLO_WORLD_DESC]]></message>
    </layout>
</metadata>
```

## Testing the Component

Just as before, zip up your new component version and upload it into
your Joomla! install's admin panel. Once it has installed, go around and
make sure that your pages and menu link appear exactly as they did
before. If everything is working properly, there should be no change at
all.

<a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Menu_Type_to_the_Site_Part"
id="content-button" class="button expand success">Prev: Adding a Menu
Type to the Site Part</a> <a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Model_to_the_Site_Part"
id="content-button" class="button expand">Next: Adding a Model to the
Site Part</a>
