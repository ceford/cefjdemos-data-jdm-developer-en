<!-- Filename: J4.x:Developing_an_MVC_Component/Adding_a_View_to_the_Site_Part / Display title: Developing an MVC Component/Adding a View to the Site Part -->

Joomla!  4.x \>Tutorial Adding a View to the Site Part

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

## Adding the View

Though a component is split into "admin" and "site" parts, the process
of adding a view is very similar in both. Just as in the basic view we
made in the last article, we will require a controller, a view, and a
template.

To get started, let's first create the new files for the site page. As
before, the source code for each file can be found under [File
Details](#File_Details).

|     |                                                                                                       |                                                       |
|-----|-------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| 1   | Create: [site/src/Controller/DisplayController.php](#site.2Fsrc.2FController.2FDisplayController.php) | The default MVC Controller for the site part          |
| 2   | Create: [site/src/View/Hello/HtmlView.php](#site.2Fsrc.2FView.2FHello.2FHtmlView.php)                 | The MVC View object for the new "Hello" page          |
| 3   | Create: [site/tmpl/hello/default.php](#site.2Ftmpl.2Fhello.2Fdefault.php)                             | The template for the new "Hello" page                 |
| 4   | Update: [helloworld.xml](#helloworld.xml)                                                             | Need to add the new files to the component's manifest |

## File Details

**site/src/Controller/DisplayController.php**

The default MVC controller in the site part. We have some basic code in
here to fetch and render a view, but as before we're mostly delegating
to the parent Joomla! class. As this is a PHP class, it lives under your
PHP namespace root `src/`, in the `Controller` namespace.

```php
<?php

namespace JohnSmith\Component\HelloWorld\Site\Controller;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Factory;

/**
 * @package     Joomla.Site
 * @subpackage  com_helloworld
 *
 * @copyright   Copyright (C) 2020 John Smith. All rights reserved.
 * @license     GNU General Public License version 3; see LICENSE
 */

/**
 * HelloWorld Component Controller
 * @since  0.0.2
 */
class DisplayController extends BaseController {
    
    public function display($cachable = false, $urlparams = array()) {        
        $document = Factory::getDocument();
        $viewName = $this->input->getCmd('view', 'login');
        $viewFormat = $document->getType();
        
        $view = $this->getView($viewName, $viewFormat);
        
        $view->document = $document;
        $view->display();
    }
    
}
```

**site/src/View/Hello/HtmlView.php**

The MVC view for this "Hello World" page. As before, this file simply
delegates to the parent object to get us started. As this is a PHP
class, it lives under `src/`, in the `View/Hello` namespace that matches
the view's name.

```php
<?php

namespace JohnSmith\Component\HelloWorld\Site\View\Hello;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\View\HtmlView as BaseHtmlView;

/**
 * @package     Joomla.Site
 * @subpackage  com_helloworld
 *
 * @copyright   Copyright (C) 2020 John Smith. All rights reserved.
 * @license     GNU General Public License version 3; see LICENSE
 */

/**
 * View for the user identity validation form
 */
class HtmlView extends BaseHtmlView {
    

    /**
     * Display the view
     *
     * @param   string  $template  The name of the layout file to parse.
     * @return  void
     */
    public function display($template = null) {
        // Call the parent display to display the layout file
        parent::display($template);
    }

}
```

**site/tmpl/hello/default.php**

The page template for our "Hello World" page in the site part. This is identical to the template we used in the admin part, to get us started. As a template, it lives under tmpl in a folder that matches the view's name.

```php
<?php

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
<h2>Hello world!</h2>
```

**helloworld.xml**

Finally, let's update the component's manifest to include the new files.
We need to tell Joomla! that the new files exist so that it will copy
them into place. We'll also update the extension's version number in the
manifest - right now, this has no real effect, but in the future
changing the version number will have more significance so it's a good
habit to get into.

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
    <version>0.0.2</version>
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
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>

    <administration>
        <!-- The link that will appear in the Admin panel's "Components" menu -->
        <menu link="index.php?option=com_helloworld">Hello World</menu>
        <!-- List of files and folders to copy, and where to copy them -->
        <files folder="admin/">
            <folder>services</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
    </administration>

</extension>
```

## Updating the Extension

To test the changes to your extension, first you will need to zip up the
`com_helloworld` folder and install it just as you did before. We'll go
through the process step-by-step one more time here:

1.  Using your web browser, navigate to the Administrator panel of your
    Joomla! site. The address would be `/administrator/`. For example:
    <a href="http://localhost/joomla/administrator/" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener"><code>http://localhost/joomla/administrator/</code></a>
2.  On the left menu, click the "System" link.
3.  On the "Install" card, click "Extensions".
4.  On the "Upload Package File" tab, browse for and select the .zip
    file you just created.

Once your new component version is installed, click the "System" link on
the main menu again, then select "Extensions" on the "Manage" card this
time. A list of the installed Joomla! extensions should appear. Type
"hello" into the search bar at the top right and hit return. You should
see your "Hello World" extension, now listed as version 0.0.2.

<img
src="https://docs.joomla.org/images/thumb/5/58/Joomla_4.x_Component_Tutorial_0.0.2_Component_Listing.png/300px-Joomla_4.x_Component_Tutorial_0.0.2_Component_Listing.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/5/58/Joomla_4.x_Component_Tutorial_0.0.2_Component_Listing.png/450px-Joomla_4.x_Component_Tutorial_0.0.2_Component_Listing.png 1.5x, https://docs.joomla.org/images/thumb/5/58/Joomla_4.x_Component_Tutorial_0.0.2_Component_Listing.png/600px-Joomla_4.x_Component_Tutorial_0.0.2_Component_Listing.png 2x"
data-file-width="1413" data-file-height="173" width="300" height="37"
alt="The Hello World component, listed in the Joomla! extensions directory." />

## Testing the New Page

The new page has no menu link yet (we will add that in the next article)
so to test it we'll need to use a direct URL. Go to
`/index.php?option=com_helloworld&view=hello` (not inside the
`/administrator` path), and you should see your new "Hello World" site
page appear.

<img
src="https://docs.joomla.org/images/thumb/a/ab/Joomla_4.x_Component_Tutorial_0.0.2_Hello_World.png/300px-Joomla_4.x_Component_Tutorial_0.0.2_Hello_World.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/a/ab/Joomla_4.x_Component_Tutorial_0.0.2_Hello_World.png/450px-Joomla_4.x_Component_Tutorial_0.0.2_Hello_World.png 1.5x, https://docs.joomla.org/images/thumb/a/ab/Joomla_4.x_Component_Tutorial_0.0.2_Hello_World.png/600px-Joomla_4.x_Component_Tutorial_0.0.2_Hello_World.png 2x"
data-file-width="1157" data-file-height="562" width="300" height="146"
alt="The Hello World page as it appears on Joomla 4 Beta 5." />

Next, we will add menu configuration for this page, so that it can be
used inside the Joomla! menu system.

<a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Developing_a_Basic_Component"
id="content-button" class="button expand success">Prev: Developing a
Basic Component</a> <a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Menu_Type_to_the_Site_Part"
id="content-button" class="button expand">Next: Adding a Menu Type to
the Site Part</a>
