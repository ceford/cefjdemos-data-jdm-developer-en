<!-- Filename: J4.x:Developing_an_MVC_Component/Developing_a_Basic_Component / Display title: Developing an MVC Component/Developing a Basic Component -->

Joomla!  4.x \>Tutorial Developing a Basic Component

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

## Your First Component

In this article we will cover how to create and install a basic Joomla!
component, which we will inventively call the "Hello World" component.

To begin, you must first use your preferred file manager to create a
directory for the Hello World! component. This directory can be anywhere
on your file system, as long as it is outside of your Joomla!
installation directory. For this example we will name the directory
`com_helloworld` (The `com` prefix means "component" and the
`helloworld` is the name of our component.). This is a great way of
managing multiple projects on the go! Technically we can name this
folder anything we want but it is always a good idea to use meaningful
names!

Next, inside this directory we need to create some files. Since this
component will have both a front end (visible to website visitors) and a
back end (used by site administrators) we will need two sub-folders.
Create a sub-folder called 'site' and one called 'admin'. Again we can
call these anything we want as we will tell Joomla! where are files are
and where they go within the Joomla! installation in our manifest file
that we create. Using your preferred file manager, create the following
files; as you create the files, add the source code for each file which
can be found in [File Details](#File_Details).

|     |                                                                                                   |                                                                       |
|-----|---------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| 1   | *[helloworld.xml](#helloworld.xml)*                                                               | An XML manifest file that tells Joomla! how to install our component. |
| 2   | *[admin/services/provider.php](#admin.2Fservices.2Fprovider.php)*                                 | Tells Joomla! how to initialise the component                         |
| 3   | *[admin/src/Controller/DisplayController.php](#admin.2Fsrc.2FController.2FDisplayController.php)* | The MVC Controller for the "Hello World" page we'll start with.       |
| 4   | *[admin/src/View/Hello/HtmlView.php](#admin.2Fsrc.2FView.2FHello.2FHtmlView.php)*                 | The MVC View for the "Hello World" page we'll start with.             |
| 5   | *[admin/tmpl/hello/default.php](#admin.2Ftmpl.2Fhello.2Fdefault.php)*                             | The HTML/PHP template for the "Hello World" page                      |

## File Details

**helloworld.xml**

This is a manifest file, which describes the component and its
configuration to Joomla! It is utilised during installation to copy the
component's files into the correct locations, trigger database
installation, configure the component's PHP namespace, and so on.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
<!-- 'version' attribute for extension tag is no longer used -->

    <name>Hello World</name>
    <!-- The following elements are optional and free of formatting constraints -->
    <creationDate>December 2020</creationDate>
    <!-- Dummy author, feel free to replace anywhere you see it-->
    <author>John Smith</author>
    <authorUrl>https://smith.ca</authorUrl>
    <copyright>John Smith</copyright>
    <license>GPL v3</license>
    <!--  The version string is recorded in the components table -->
    <version>0.0.1</version>
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
            
    <administration>
        <!-- The link that will appear in the Admin panel's "Components" menu -->
        <menu link="index.php?option=com_helloworld">Hello World</menu>
        <!-- List of files and folders to copy. Note the 'folder' attribute.
             This is the name of the folder in your component package to copy FROM -->
        <files folder="admin/">
            <folder>services</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
    </administration>

</extension>
```       

**admin/services/provider.php**

This is a special file that tells Joomla! how to initialise the
component - which services it requires and how they should be provided.

```php
<?php

defined('_JEXEC') or die;

use Joomla\CMS\Dispatcher\ComponentDispatcherFactoryInterface;
use Joomla\CMS\Extension\ComponentInterface;
use Joomla\CMS\Extension\MVCComponent;
use Joomla\CMS\Extension\Service\Provider\ComponentDispatcherFactory;
use Joomla\CMS\Extension\Service\Provider\MVCFactory;
use Joomla\CMS\MVC\Factory\MVCFactoryInterface;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

return new class implements ServiceProviderInterface {
    
    public function register(Container $container): void {
        $container->registerServiceProvider(new MVCFactory('\\JohnSmith\\Component\\HelloWorld'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\JohnSmith\\Component\\HelloWorld'));
        $container->set(
            ComponentInterface::class,
            function (Container $container) {
                $component = new MVCComponent($container->get(ComponentDispatcherFactoryInterface::class));
                $component->setMVCFactory($container->get(MVCFactoryInterface::class));

                return $component;
            }
        );
    }
};
```

**admin/src/Controller/DisplayController.php**

The MVC controller for our first view, "hello". As our component grows
in complexity, a page's controller will handle model fetching, form
submissions, and so on. Here, it simply sets its default view and leaves
the rest to its parent.

```php
<?php

namespace JohnSmith\Component\HelloWorld\Administrator\Controller;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\Controller\BaseController;

/**
 * @package     Joomla.Administrator
 * @subpackage  com_helloworld
 *
 * @copyright   Copyright (C) 2020 John Smith. All rights reserved.
 * @license     GNU General Public License version 3; see LICENSE
 */

/**
 * Default Controller of HelloWorld component
 *
 * @package     Joomla.Administrator
 * @subpackage  com_helloworld
 */
class DisplayController extends BaseController {
    /**
     * The default view for the display method.
     *
     * @var string
     */
    protected $default_view = 'hello';
    
    public function display($cachable = false, $urlparams = array()) {
        return parent::display($cachable, $urlparams);
    }
    
}
```

**admin/src/View/Hello/HtmlView.php**

This is the MVC view object for our first page. The job of a view object is to handle the setup work for a view template - selecting a layout, fetching JavaScript, generating toolbars, etc. To simply get our "hello world" page to load, we will delegate the work of initialising the view to Joomla!

```php
<?php

namespace JohnSmith\Component\HelloWorld\Administrator\View\Hello;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\View\HtmlView as BaseHtmlView;

/**
 * @package     Joomla.Administrator
 * @subpackage  com_helloworld
 *
 * @copyright   Copyright (C) 2020 John Smith. All rights reserved.
 * @license     GNU General Public License version 3; see LICENSE
 */

/**
 * Main "Hello World" Admin View
 */
class HtmlView extends BaseHtmlView {
    
    /**
     * Display the main "Hello World" view
     *
     * @param   string  $tpl  The name of the template file to parse; automatically searches through the template paths.
     * @return  void
     */
    function display($tpl = null) {
        parent::display($tpl);
    }

}
```

**admin/tmpl/hello/default.php**

This file holds the template for the page. It is used to render the page utilising the setup done by the view object.

When no specific layout is requested for a view, Joomla! will load the template in the default.php file, which is why we're using that filename.

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

## Installing the Component

Using your preferred file manager, create a .zip file of the
`com_helloworld` directory - include the directory itself and make sure
to preserve the directory structure.

Now we need to install the component. Follow this process:

1.  Using your web browser, navigate to the Administrator panel of your
    Joomla! site. The address would be `/administrator/`. For example:
    <a href="http://localhost/joomla/administrator/" class="external free"
    target="_blank"
    rel="nofollow noreferrer noopener"><code>http://localhost/joomla/administrator/</code></a>
2.  On the left menu, click the "System" link.
3.  On the "Install" card, click "Extensions".
4.  On the "Upload Package File" tab, browse for and select the .zip
    file you just created.

The page should automatically process the installation of the component,
and you will receive a message telling you whether it was successful or
not.

## Testing the Component

Once the component is installed, click the "Components" section of the
menu on the left of the admin panel. You should now see a new link under
this section labeled "Hello World". This is the link detailed in the
component's manifest file. If you click it, you should see the "Hello
World" page.

Congratulations! You've created your first Joomla! component. Now, let's
start making it useful.

<a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Introduction"
id="content-button" class="button expand success">Prev: Introduction</a>
<a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_View_to_the_Site_Part"
id="content-button" class="button expand">Next: Adding a View to the
Site Part</a>
