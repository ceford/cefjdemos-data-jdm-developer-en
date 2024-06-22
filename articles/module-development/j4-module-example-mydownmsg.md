<!-- Filename: J4.x:J4_Module_example_-_Mydownmsg / Display title: J4 Module example - Mydownmsg -->

## The J4xdemos-mod-mydownmsg Example Module Tutorial

There is a very good Module tutorial here:

- [J4x: Creating a Simple
  Module](https://docs.joomla.org/J4.x:Creating_a_Simple_Module "J4.x:Creating a Simple Module")

This tutorial example is simpler but provides a working example to try
out, step through with the debugger and simple code to examine.

The installable zip file is available from Github:

- <a
  href="https://github.com/ceford/j4xdemos-mod-mydownmsg/raw/master/mod_mydownmsg.zip"
  class="external free" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/ceford/j4xdemos-mod-mydownmsg/raw/master/mod_mydownmsg.zip</a>

## Purpose

One of the sites I develop for has Users all over the World and offers
16 or so languages. When I need to close the site for system maintenance
I use the Custom module to create a short message and locate it in a
position near the top of the page. The module is published an hour or so
before the event and unpublished at the end of maintenance. The snag is
that the message is in English. So this demonstration module does the
same job - displays a piece of text - but in whatever language is
appropriate for the Site User.

The module name - mydownmsg - is just a reminder that the module
displays My Site going down message.

## The zip file structure

It is usually helpful to know the zip file structure to check that the
parts are in the expected places when unpacked;

```bash
    mod_mydownmsg.zip
         mod_mydownmsg
              language
                   en-GB
                        en-GB.mod_mydownmsg.ini
                        en-GB.mod_mydownmsg.sys.ini
                   de-DE
                        de-DE.mod_mydownmsg.ini
                   fr-FR
                        fr-FR.mod_mydownmsg.ini
              tmpl
                   default.php
              mod_mydownmsg.php
              mod_mydownmsg.xml
```

Note that all of our administrators are obliged to use English so the
non-English translation files only need to translate the strings that
are seen by Site visitors.

## The Manifest File

The manifest file controls the extension installation process. Notable
features

The **extension** statement gives some essential data:

- type="module" is fairly obvious
- version="4.0" means that this extension is for Joomla 4 and should not
  install in previous versions.
- method="upgrade" means that this extension can be installed and
  upgraded if newer versions become available
- client="site" tells Joomla that this is a Site module rather than an
  Administrator module.

The **version** statement is used to control upgrading by preventing
older version overwriting newer versions.

The **config** section shows what parameters this module needs. All
should have defaults because they are set on installation. Any change to
the parameters needs the module to be re-installed from the revised zip
file. (Check!)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="module" version="4.0" method="upgrade" client="site">
	<name>mod_mydownmsg</name>
	<creationDate>August 2019</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@ford.myzen.co.uk</authorEmail>
	<authorUrl>http://www.fford.me.uk/</authorUrl>
	<copyright>Copyright (C) 2019 Clifford E Ford, All rights reserved.</copyright>
	<license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
	<!--  The version string is recorded in the components table -->
	<version>1.0</version>
	<!-- The description is optional and defaults to the name -->
	<description>MOD_MYDOWNMSG_XML_DESCRIPTION</description>
	<namespace>J4xdemos\Module\Mydownmsg</namespace>

	<files>
		<filename module="mod_mydownmsg">mod_mydownmsg.php</filename>
        	<filename>mod_mydownmsg.xml</filename>
        	<folder>tmpl</folder>
	</files>

	<languages folder="language">
		<language tag="en-GB">en-GB/en-GB.mod_mydownmsg.ini</language>
		<language tag="en-GB">en-GB/en-GB.mod_mydownmsg.sys.ini</language>
		<language tag="de-DE">de-DE/de-DE.mod_mydownmsg.ini</language>
		<language tag="fr-FR">fr-FR/fr-FR.mod_mydownmsg.ini</language>
	</languages>

	<config>
		<fields name="params">
			<fieldset name="basic">

				<field 
					name="msg_id"
					type="list"
					label="MOD_MYDOWNMSG_PARAMS_MSG_ID_LABEL"
					description="MOD_MYDOWNMSG_PARAMS_MSG_ID_DESC"
					default="v1"
					required="true"
				>
					<option value="v1">MOD_MYDOWNMSG_PARAMS_MSG_ID_OPTION_V1</option>
					<option value="v2">MOD_MYDOWNMSG_PARAMS_MSG_ID_OPTION_V2</option>
				</field>

				<field
					name="hour"
					type="number"
					label="MOD_MYDOWNMSG_PARAMS_HOUR_LABEL"
					description="MOD_MYDOWNMSG_PARAMS_HOUR_DESC"
					default="12"
					min="0"
					max="23"
					required="true"
					></field>

				<field
					name="minute"
					type="number"
					label="MOD_MYDOWNMSG_PARAMS_MINUTE_LABEL"
					description="MOD_MYDOWNMSG_PARAMS_MINUTE_DESC"
					default="00"
					min="00"
					max="59"
					required="true"
					></field>

				<field
					name="tz"
					type="number"
					label="MOD_MYDOWNMSG_PARAMS_TZ_LABEL"
					description="MOD_MYDOWNMSG_PARAMS_TZ_DESC"
					default="0"
					min="-11"
					max="11"
					required="true"
					></field>

			</fieldset>
		</fields>
	</config>
</extension>
```

## The Language Files

This module has very few strings. Those in the **sys.ini** files are
used on installation and for listing among other modules. The **.ini**
files are used in the Administrator form and in the Site rendering of
the module. Note the conventions in identifying strings:

MOD\_\[name\]\_\[purpose\]\_\[usage\]

The files below show that the German and French translations only
include the strings to be seen by site visitors as the Parameters form
will always be administered in English. In case you were not aware:
en-GB strings are always loaded first by default to ensure that
accidentally untranslated strings do not appear as string keys. The
strings of the required language are then loaded and overwrite the
equivalent English strings.

The French and German versions of the strings here were obtained using
Google Translation Tools. I am told this works better for sentences than
single words!

### en-GB/en-GB.mod_mydownmsg.ini

```ini
    MOD_MYDOWNMSG="My System Maintainence Message"
    MOD_MYDOWNMSG_XML_DESCRIPTION="Show a message about imminent system down time."

    MOD_MYDOWNMSG_MSG_V1="This site will close for a short period at %s"
    MOD_MYDOWNMSG_MSG_V2="Please log out. This site will close for one hour or more at %s"

    MOD_MYDOWNMSG_PARAMS_MSG_ID_LABEL="Select Message version"
    MOD_MYDOWNMSG_PARAMS_MSG_ID_DESC="The short downtime or long downtime message vesion."

    MOD_MYDOWNMSG_PARAMS_MSG_ID_OPTION_V1="Short < 1 hour"
    MOD_MYDOWNMSG_PARAMS_MSG_ID_OPTION_V2="Long > 1 hour"

    MOD_MYDOWNMSG_PARAMS_HOUR_LABEL="Hour"
    MOD_MYDOWNMSG_PARAMS_HOUR_DESC="Set the hour when the site will close"

    MOD_MYDOWNMSG_PARAMS_MINUTE_LABEL="Minute"
    MOD_MYDOWNMSG_PARAMS_MINUTE_DESC="Set the minutes when the site will close"

    MOD_MYDOWNMSG_PARAMS_TZ_LABEL="Time Zone"
    MOD_MYDOWNMSG_PARAMS_TZ_DESC="Set your time zone offset from GMT"
```

### fr-FR/fr-FR.mod_mydownmsg.ini

```ini
    MOD_MYDOWNMSG_MSG_V1="Ce site sera fermé pour une courte période à %s"
    MOD_MYDOWNMSG_MSG_V2="Veuillez vous déconnecter. Ce site sera fermé pendant une heure ou plus à %s"
```

### de-DE/de-DE.mod_mydownmsg.ini

```ini
    MOD_MYDOWNMSG_MSG_V1="Diese Seite wird für kurze Zeit um %s geschlossen"
    MOD_MYDOWNMSG_MSG_V2="Bitte melden Sie sich ab. Diese Seite wird für eine Stunde oder länger um %s geschlossen"
```

## The Module Code

The module code is incredibly simple. There are two files:

- mod_mydownmsg.php simply tells Joomla which template file to use,
  allowing template developers to override the default template.
- tmpl/default.php contains the code to render the output.

This module is so simple that it does not need anything else!

### mod_mydownmsg.php

```php
<?php
/**
 * @package     J4xdemos.Module
 * @subpackage  mod_mydownmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Helper\ModuleHelper;

// get the message option

require ModuleHelper::getLayoutPath('mod_mydownmsg', $params->get('layout', 'default'));
```

### tmpl/default.php

The default template fetches the module parameters and assembles a
string for output using the string fetched from the translation file.

```php
<?php
/**
 * @package     J4xdemos.Module
 * @subpackage  mod_mydownmsg
 *
 * @copyright   Copyright (C) 2019 Clifford E Ford. All rights reserved.
 * @license     GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

// the $msg string contains a %s placeholder to be replaced in a sprintf statement
$msg = Text::_('MOD_MYDOWNMSG_MSG_' . strtoupper($params->get('msg_id')));

$tod = $params->get('hour') . ':' . $params->get('minute');
$tz = $params->get('tz');

if ($tz > 0 )
{
	$tz = '(+' . $tz . ')';
}
else if ($tz < 0)
{
	$tz = '(-' . $tz . ')';
}
else {
	$tz = '';
}

$tod .= ' GMT ' . $tz;

?>

<div class="alert alert-warning" role="alert">
	<?php echo sprintf ($msg, $tod); ?>
</div>
```

## Installation

Installation by Upload Package File in the Install / Extensions menu
seems to work. That done ...

In the Module edit form:

1.  enter a title. Remember that a module may have more than one
    instance so they may appear in different locations or have different
    parameters.
2.  set the time when the site will go down.
3.  set the time zone (there is probably a better way than using GMT +-
    n hours).

in the right hand common parameters list

1.  set the **Title** to **hide**
2.  select a template **Position** - in Cassiopeia **top-a** puts the
    message above the page content, perfect.
3.  set the **Status** to **Published**
4.  in the **Menu Assignment** tab select **On all pages**
5.  **Save** and you are ready to check the Site appearance

Bug note: this module should be first in the list of modules in position
top-a. However, the Ordering selector has no effect and there is no
First position to select. When the module list is filtered on position
top-a this item can be dragged to the top of the list but it does not
stick.

<img
src="https://docs.joomla.org/images/d/d6/Module-parameters-form.jpg"
decoding="async" data-file-width="518" data-file-height="677"
width="518" height="677" alt="Module Parameters form" />

You can now check the message as seen by Site visitors:

<img src="https://docs.joomla.org/images/8/87/Site-message-english.jpg"
decoding="async" data-file-width="410" data-file-height="306"
width="410" height="306" alt="Site Message in English" />

## Testing the other languages

To test the French and German versions of the site message, go via
System / Manage / Languages and set the default language to French or
German. Voila:

The site message in French:

<img src="https://docs.joomla.org/images/e/ec/Site-message-french.jpg"
decoding="async" data-file-width="447" data-file-height="314"
width="447" height="314" alt="Site message in French" />

The site message in German:

<img src="https://docs.joomla.org/images/8/80/Site-message-german.jpg"
decoding="async" data-file-width="453" data-file-height="312"
width="453" height="312" alt="Site message in German" />

Clifford E Ford, August 2019
