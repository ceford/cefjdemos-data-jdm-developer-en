<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: Example: Site Down Message -->

## Introduction

This article presents a complete Joomla site module for new developers to take apart to see its structure and how it works. It is a replacement for a previous module that used older coding conventions. This one is designed for Joomla 5 and later.

There are introductory articles on *General Concepts* and *Modules* in the Joomla Programmers Documentation and reproduced in Jdocmanual for your convenience. This article is similar to the JPD [Basic Module](jdocmanual?article=docus/modules/basic-module) but has some extra functionality.

## Purpose

The module is designed to display a temporary message in one of several languages for a short period before the site closes for system maintenance. That gives users the opportunity to finish what they are doing before the closure occurs.

The module name is `mod_downmsg` and the message it displays in English is similar to *This site will close for a short period at 12:0 GMT*. The time and message can be selected to suit the impending site closure.

The module can be downloaded from GitHub to install or examine as appropriate.

## Source Code Structure

The following is a schematic illustration of the structure of the source code used to create the extension:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- mod_downmsg (folder compressed to create an installable zip file)
    |-- help
        |-- en-GB
            |-- downmsg.html (this is Help screen used in the backend module edit form)
    |-- language
        |-- de-DE
            |-- mod_downmsg.ini (only frontend strings are included)
        |-- en-GB (language folder, kept with the extension code)
            |-- mod_downmsg.ini
            |-- mod_downmsg.sys.ini
        |-- fr-FR
            |-- mod_downmsg.ini (only frontend strings are included)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Dispatcher (folder for extension specific code)
            |-- Dispatcher.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- mod_downmsg.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

This is the structure as seen in the VSCode or VSCodium IDE:

![Plugin development file structure in vscodium](../../../en/images/modules/downmsg-module-vscodium.png)

## The Manifest File

The manifest file controls the extension install, update and uninstall processes:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" method="upgrade" client="site">
    <name>mod_downmsg</name>
    <creationDate>December 2024</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@xxx.yyy.co.uk</authorEmail>
    <authorUrl>http://jdocmanual.org/</authorUrl>
    <copyright>Copyright (C) 2024 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.1.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>MOD_DOWNMSG_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Module\Downmsg</namespace>
    <files>
        <folder>help</folder>
        <folder>language</folder>
        <folder module="mod_downmsg">services</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    <help url="MOD_DOWNMSG_HELP_URL" />
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Mod Downmsg Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-mod-downmsg/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="msg_id"
                    type="list"
                    label="MOD_DOWNMSG_PARAMS_MSG_ID_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MSG_ID_DESC"
                    default="v1"
                    required="true"
                >
                    <option value="v1">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
                    <option value="v2">MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
                </field>
                <field
                    name="hour"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_HOUR_LABEL"
                    description="MOD_DOWNMSG_PARAMS_HOUR_DESC"
                    default="12"
                    min="0"
                    max="23"
                    required="true"
                />
                <field
                    name="minute"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_MINUTE_LABEL"
                    description="MOD_DOWNMSG_PARAMS_MINUTE_DESC"
                    default="00"
                    min="00"
                    max="59"
                    required="true"
                />
                <field
                    name="tz"
                    type="number"
                    label="MOD_DOWNMSG_PARAMS_TZ_LABEL"
                    description="MOD_DOWNMSG_PARAMS_TZ_DESC"
                    default="0"
                    min="-11"
                    max="11"
                    required="true"
                />
            </fieldset>
        </fields>
    </config>
</extension>
```

### Element Notes

- The **extension** attributes:
    - **type="module"** indicates the type of extension.
    - **method="upgrade"** means that this extension can be installed and upgraded if newer versions become available.
    - **client="site"** tells Joomla that this is a *Site* module rather than an *Administrator* module.
- The **version** statement is used to control upgrading. If an update server is available it is compared with the version recorded there. It also prevents an older version overwriting a newer version.
- The **namespace** path attribute tells Joomla where to look for namespace classes.
- The **files** section lists folders and files for installation. If an item is not present here it will not be installed even if it is present in the zipped install file.
- The **help** url attribute allows a custom help file to be used in the module edit form. The path to the help file is a value for the given key in the en-GB/mod_downmsg.ini file.
- The **config** section shows what parameters this module needs. All should have defaults because they are set on installation.

## The Language Files

This module has very few strings. Those in the **sys.ini** files are used on installation and for listing among other modules. The **.ini** files are used in the Administrator form and in the Site rendering of the module. Note the conventions in identifying strings:

MOD\_\[name\]\_\[purpose\]\_\[usage\]

The files below show that the German and French translations only include the strings to be seen by site visitors as the Parameters form will always be administered in English. In case you were not aware: en-GB strings are always loaded first by default to ensure that accidentally untranslated strings do not appear as string keys. The strings of the required language are then loaded and overwrite the equivalent English strings.

The French and German versions of the strings here were obtained using Google Translation Tools. This may work better for sentences than single words!

### en-GB/mod_downmsg.ini

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"

MOD_DOWNMSG_MSG_V1="This site will close for a short period at %s"
MOD_DOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

MOD_DOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
MOD_DOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
MOD_DOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

MOD_DOWNMSG_PARAMS_HOUR_LABEL="Hour"
MOD_DOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

MOD_DOWNMSG_PARAMS_MINUTE_LABEL="Minute"
MOD_DOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

MOD_DOWNMSG_PARAMS_TZ_LABEL="Time Zone"
MOD_DOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### en-GB/mod_downmsg.sys.ini

This file is used for system management.

```ini
MOD_DOWNMSG="System Down Message"
MOD_DOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."
```

### de-DE/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
MOD_DOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

### fr-FR/mod_downmsg.ini

```ini
MOD_DOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
MOD_DOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
MOD_DOWNMSG_HELP_URL="../modules/mod_downmsg/help/en-GB/downmsg.html"
```

## The Module Code

The module code is incredibly simple. There are three PHP files:

- services/provider.php (the dependency injection code).
- src/Dispatcher/Dispatcher.php (assembles data to be used for display).
- tmpl/default.php (the template to display the data, which may be overridden).

### services/provider.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\Service\Provider\Module;
use Joomla\CMS\Extension\Service\Provider\ModuleDispatcherFactory;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The module Downmsg service provider.
 *
 * @since  4.4.0
 */
return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.4.0
     */
    public function register(Container $container): void
    {
        $container->registerServiceProvider(new ModuleDispatcherFactory('\\Cefjdemos\\Module\\Downmsg'));
        $container->registerServiceProvider(new Module());
    }
};
```

### src/Dispatcher/Dispatcher.php

```php
<?php

/**
 * @package     Cefjdemos.Site
 * @subpackage  mod_downmsg
 *
 * @copyright   (C) 2023 Open Source Matters, Inc. <https://www.joomla.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Cefjdemos\Module\Downmsg\Site\Dispatcher;

use Joomla\CMS\Dispatcher\AbstractModuleDispatcher;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Dispatcher class for mod_downmsg
 *
 * @since  4.4.0
 */
class Dispatcher extends AbstractModuleDispatcher
{
    /**
     * Returns the layout data.
     *
     * @return  array
     *
     * @since   4.4.0
     */
    protected function getLayoutData()
    {
        $data = parent::getLayoutData();

        return $data;
    }
}
```

### tmpl/default.php

```php
<?php

/**
 * @package     Cefjdemos.Module
 * @subpackage  mod_downmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_DOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0) {
    $tz = '(+' . $tz . ')';
} elseif ($tz < 0) {
    $tz = '(-' . $tz . ')';
} else {
    $tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning text-center" role="alert">
    <?php echo sprintf($msg, $tod); ?>
</div>
```

## Installation

Form the Administrator menu, navigate to the System / Install / Extensions page and use the Upload Package File tab to upload the zip file. Then navigate to Content / Site Modules and edit the newly installed module.

In the Module edit form:

1. Enter a title. Remember that a module may have more than one instance so they may appear in different locations or have different parameters.
2. Set the time when the site will go down.
3. Set the time zone (there is probably a better way than using GMT +- n hours).

In the right hand common parameters list

1.  Set the **Title** to **hide**.
2.  Select a template **Position** - in Cassiopeia **topbar** puts the message above the page content, perfect.
3.  Set the **Status** to **Published**.
4.  Nn the **Menu Assignment** tab select **On all pages**.
5.  **Save** and you are ready to check the Site appearance.

![the module edit form](../../../en/images/modules/downmsg-module-edit-form.png)

## Testing

This is how the message appears in English:

![site down message in english](../../../en/images/modules/downmsg-module-result-en.png)

In German:

![site down message in english](../../../en/images/modules/downmsg-module-result-de.png)

And French:

![site down message in english](../../../en/images/modules/downmsg-module-result-fr.png)

## Update Site and Change Log

These items are included in the source but are not covered here.
