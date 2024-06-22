<!-- Filename: Manifest_files / Display title: Manifest files -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5

Within Joomla there are manifest files for all of the extensions. These
files include the general installation information as well as parameters
for the configuration of the
[extension](https://docs.joomla.org/extension "Special:MyLanguage/extension")
itself. Since Joomla! 2.5, there are very few differences between the
manifest file formats for the different [types of
extensions](https://docs.joomla.org/Extension_types_(technical_definitions) "Special:MyLanguage/Extension types (technical definitions)"),
allowing each type to access the full power of the Joomla! installer.

## Naming conventions

The file must be named `manifest.xml` (only Joomla versions 2.5 and 3!)
or `.xml` (Joomla versions 2.5, 3 and 4) and located in the root
directory of the installation package.

Joomla 4.x: Automatic namespace mapping will fail if a manifest file
named `manifest.xml` is used. See:
<a href="https://github.com/joomla/joomla-cms/issues/37750"
class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://github.com/joomla/joomla-cms/issues/37750</a>

## Syntax

### Root element

The primary tag of the installation file is:

This starting and closing tag is the same for all extensions. The
following attributes are allowed within the tag:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribute</th>
<th>Values</th>
<th>Applicable to</th>
<th>Description</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>component</code><br />
<code>file</code><br />
<code>language</code><br />
<code>library</code><br />
<code>module</code><br />
<code>package</code><br />
<code>plugin</code><br />
<code>template</code><br />
<code>element</code></td>
<td>All extensions</td>
<td>This attribute describes the type of the extension for the
installer. Based on this type further requirements to sub-tags
apply.</td>
</tr>
<tr class="even">
<td>version</td>
<td><code>2.5</code><br />
<code>3.0</code></td>
<td>All extensions</td>
<td>String that identifies the version of Joomla for which this
extension is developed. This is not used in <img
src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" /> and has been removed from <img
src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> and higher.</td>
</tr>
<tr class="odd">
<td>method</td>
<td><code>install</code><br />
<code>upgrade</code></td>
<td>All extensions</td>
<td>The default value <code>install</code> will be also used if the
method attribute is not used. The <code>install</code> value means the
installer will gracefully stop if it finds any existing file/folder of
the new extension.</td>
</tr>
<tr class="even">
<td>client</td>
<td><code>site</code><br />
<code>administrator</code></td>
<td>Modules</td>
<td>The client attribute allows you to specify for which application
client the new module is available.</td>
</tr>
<tr class="odd">
<td>group</td>
<td><em>string</em></td>
<td>Plugins</td>
<td>The group name specifies for which group of plugins the new plugin
is available. The existing groups are the folder names within the
directory <code>/plugins</code>. The installer will create new folder
names for group names that do not exist yet.</td>
</tr>
</tbody>
</table>

### Metadata

The following elements can be used to insert metadata. None of these
elements are required; if they are present, they must be a child of the
root element.

     – raw component name (e.g. com_banners). 
     – author's name (e.g. Joomla! Project)
     – date of creation or release (e.g. April 2006)
     – a copyright statement (e.g. (C) 2005 - 2011 Open Source Matters. All rights reserved.)
     – a license statement (e.g. NU General Public License version 2 or later; see LICENSE.txt)
     – author's email address (e.g. admin@joomla.org)
     – URL to the author's website (e.g. www.joomla.org)
     – the version number of the extension (e.g. 1.6.0)
     – the description of the component. This is a translatable field. (e.g. COM_BANNERS_XML_DESCRIPTION)
     – the internal name of the component. If omitted, name will be cleaned and used

Note: The and tags are also translatable fields so that the name and
description of the extension can be shown to the user in their native
language.

### Front-end files

```xml
	<files folder="from-folder">
		<filename>example.php</filename>
		<folder>examples</folder>
	</files>
```  

Files to copy to the front-end directory should be placed in the
element. You can use the optional `folder` attribute to specify a
directory **in the ZIP package** to copy **from**. Each file to copy
must be represented by a element. If you want to copy an entire folder
at once, you can define it as a .

For **plugins**, the raw name of the plugin should be placed in the
`plugin` attribute on the element that points to the file containing the
plugin's class. For example, in the case of a system plugin called
"example" (full name `plg_system_example`), use `example.php`.

### Media files

```xml
	<media folder="media" destination="com_example">
		<filename>com_example_logo.png</filename>
		<folder>css</folder>
		<folder>js</folder>
	</media>
```

This example will copy the file(s) (`/media/com_example_logo.png`) and
folders ( `/media/css/` and `/media/js/` ) listed to
`/media/com_example/`, creating the `com_example` folder if required.
You can use the optional `folder` attribute to specify a directory **in
the ZIP package** to copy **from** (in this case, `media`).

Extensions should be storing assets they need to be web accessible (JS,
CSS, images etc) in `media`. Amongst other things this feature was added
as step in the progression to multi-site support and the eventual move
of code files (PHP) out of the web accessible areas of the server.  
Note: the media section is not parsed for 'package' type extensions.

Ref:

- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/4CAASJqFY-k/PvPj14gP29EJ"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>
- <a
  href="https://groups.google.com/forum/#!msg/joomla-dev-cms/uNmhX98sKbE/p8p68Jke680J"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Google Groups - joomla-dev-cms
  thread</a>

### Administration section

```xml
	<administration>
		<!-- various elements -->
	</administration>
```

The administration section is defined in the element. Since only
[components](https://docs.joomla.org/Component "Special:MyLanguage/Component")
apply to both the
[administrator](https://docs.joomla.org/Site_(Application) "Special:MyLanguagehttps://docs.joomla.org/Administrator_(Application)"),
**only component manifests can include this element**.

#### Back-end Files

Files to copy to the back-end directory should be placed in the element
under the . You can use the optional `folder` attribute to specify a
directory **in the ZIP package** to copy **from**. See *Front-end files*
for further rules.

#### Menu Links and Submenus

**Version Note:** Prior to Joomla 3.4, not having a tag in your manifest 
XML file still led to a menu item being created.
This bug was fixed in Joomla 3.4, so if there is no tag in your manifest 
file, then no admin menu item is created for the component.

```xml
	<menu>COM_EXAMPLE</menu>
	<submenu>
		<!--
			Note that all & must be escaped to &amp; for the file to be valid
			XML and be parsed by the installer
		-->
		<menu link="anoption=avalue&amp;anoption1=avalue1">COM_EXAMPLE_SUBMENU_ANOPTION</menu>
		<menu view="viewname">COM_EXAMPLE_SUBMENU_VIEWNAME</menu>
	</submenu>
```

The text for the main menu item for the component is defined in the `<menu>`
item, a child of `<administration>`. A `<submenu>` element may also be 
present (also a child of `<administration>`), which may contain more 
menu items defined by `<menu>`.

Additionally, each `<menu>` item can define the following attributes:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribute</th>
<th>Description</th>
</tr>
&#10;<tr class="odd">
<td>link</td>
<td>A link to send the user to when the menu item is clicked. You can
use "view" instead.</td>
</tr>
<tr class="even">
<td>img</td>
<td>The (relative) path to an image (16x16 pixels) to appear beside the
menu item.
<p><u>Must be an url compatible as a file too (e.g. no
spaces) !</u></p></td>
</tr>
<tr class="odd">
<td>alt</td>
<td></td>
</tr>
<tr class="even">
<td>view</td>
<td>An URL parameter to add to the link. For example,
COM_EXAMPLE
in com_example's XML manifest would cause the URL of the menu item to be
<code>index.php?option=com_example&amp;view=cpanel</code>. You can use
"link" instead.</td>
</tr>
</tbody>
</table>

The value inside the tag is the menu's label. Unlike Joomla! 1.5, you
can not use a natural language string. For example, if you would enter
"Example Component" instead of COM_EXAMPLE, it would result in your
component name appearing as example-component in the menu and you would
be unable to provide a translation. In order to provide a translation
you need to create a file named en-GB.com_example.sys.ini in
administrator/languages/en-GB (you can use the manifest's tag to copy it
during installation) or in
administrator/components/com_example/language/en-GB. In the latter case,
you must not include the translation file in the tag. As long as you
have placed the language directory in your tag, it will be copied along
when the component is being installed.

The contents of that file should be:

```ini
    COM_EXAMPLE="Example Component"
    COM_EXAMPLE_SUBMENU_ANOPTION="Another Option"
    COM_EXAMPLE_SUBMENU_VIEWNAME="Another View"
```

Please note that the language string must be enclosed in double quotes,
as per Joomla!'s translation standards.

Joomla! 1.6 and later sorts the Component menu items based on the actual
translation of the key you supply in your XML manifest. This means that
the sorting order is correct no matter what you call your translation
key and no matter which language the site is being displayed in.
Essentially, Joomla! 1.6 fixed the wrong sorting of the Components menu
experienced under Joomla! 1.5 for the majority (non-English speaking!)
of Joomla! users.

### Dashboards

Specifies the details for displaying a dashboard for the component in
the Administrator area for the site.

- It will make a dashboard icon appear next to the administrator menu
  item for the component
- The dashboard icon will click through to display modules assigned to
  the cpanel-example administrator module position
- The title and icon defined in the XML file will be used as the header
  and icon at the top of the component's dashboard page.

```xml
  <dashboards>
		<dashboard title="COM_EXAMPLE_DASHBOARD_TITLE" icon="icon-lock">example</dashboard>
	</dashboards>  
```

### Configuration

<img
src="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/25px-Stop_hand_nuvola.svg.png"
decoding="async"
srcset="https://docs.joomla.org/images/thumb/4/41/Stop_hand_nuvola.svg.png/38px-Stop_hand_nuvola.svg.png 1.5x, https://docs.joomla.org/images/4/41/Stop_hand_nuvola.svg.png 2x"
data-file-width="40" data-file-height="40" width="25" height="25"
alt="Stop hand nuvola.svg.png" />Warning!

Components **do not support** configuration definitions **in the
manifest**. This was a way implemented in Joomla! 1.5. They can define
configuration options for multiple levels using the config.xml file. For
how to add this to your component read the [Developing an MVC Component
tutorial](https://docs.joomla.org/J3.x:Developing_an_MVC_Component/Adding_configuration "Special:MyLanguage/J3.x:Developing an MVC Component/Adding configuration").

The `<config>` element, a child of the root, describes the configuration options
for the extension. If applicable, the options will be shown by the
appropriate Manager (Plugin Manager, Module Manager or Template
Manager). **Configuration options for Components are defined in a
separate file named `config.xml`. Its root element should be `<config>`, plugins
and modules use the `<config>` section in the extension manifest file.**

Each fieldset must contain one or more elements, each representing a
single [form
field](https://docs.joomla.org/form_field "Special:MyLanguage/form field")
with a label. See [Standard form field
types](https://docs.joomla.org/Standard_form_field_types "Special:MyLanguage/Standard form field types")
for a list of allowed form field types and example XML form field
definitions.

### Namespace

Specify the namespace to be used for autoloading the plugin. The given
namespace will autoload to the root directory of your extension by
default however you can optionally add a "path" attribute to the
namespace element to specify a subpath within your extensions root
directory.

### SQL

```sql
    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.install.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/example.uninstall.sql</file>
        </sql>
    </uninstall>
```

In the above example, we put the SQL files in the `admin/sql` folder of
the installation package. You have to include the `sql` folder in the
administration files (as described in *Back-end files*).

You can execute SQL during the installation and/or uninstallation using
the and elements, respectively. A element should appear as a child of
these elements. can contain any number of elements, each defining a
single SQL file to execute. Their database driver types are described by
the `driver` attribute, their character sets by the `charset` attribute.

#### Update of the SQL Schema

Since 1.6, there is also an tag, which allows you to provide a series of
SQL files to update the current schema.

```sql
	<update>
		<schemas>
			<schemapath type="mysql">sql/updates/mysql</schemapath>
			<schemapath type="sqlsrv">sql/updates/sqlsrv</schemapath>
		</schemas>
	</update>
```  

For example, in order to go from version `1.0.0` to version `1.0.1` in a
**MySQL** database, a `1.0.1.sql` file must be created inside the
`sql/updates/mysql` folder and the tag of the manifest must be updated
to

```sql
<version>1.0.1</version>
```
  
The final structure of the sql folder will look like this (assuming a
**MySQL** database)

    sql
     |-->example.install.sql
     |-->example.uninstall.sql
     |-->updates
         |-->mysql
            |-->1.0.1.sql

Similar files must be created for subsequent versions.

### Language Files

Since Joomla! 1.5, extension developers had to put extension language
files in the Joomla! main language folder using the ... tag as shown
below. **This method can still be used all Joomla! versions**.

```xml
<!-- Joomla! language tag -->
<languages folder="langfiles">
	<language tag="en-GB">en-GB.com_example.ini</language>
</languages>
```

However since Joomla! 1.6 it is recommended that you place your
extension's language files in your extension folder. Joomla! will then
automatically load your extension's language files.

By storing extension language files in the extension folder, you benefit
by isolating and protecting your extension's language files. For
example, an administrator removes a language from their Joomla!
installation. Your extension's language files will not be removed. They
will remain in place and will be available if the language is installed
again.

The structure of the language folder for frontend and backend is the
same. You put them in the language tag (e.g. **en-GB** ) of each
language in your language folder, i.e. **language/en-GB/**. You have to
specify those folders in the front-end and back-end files too.

In your manifest, simply include the '**language'** folder in your files
section. The sub-directories for each language will automatically be
copied. Inside the group, add a element alongside the items in the group
as shown in this example:

```xml
<files>
	<filename plugin="alpha">alpha.php</filename>
	<folder>sql</folder>
	<folder>language</folder>
</files>
```

Note that both ways can work together. Here is an example from core:

```xml
<files>
	<filename plugin="languagecode">languagecode.php</filename>
	<filename>index.html</filename>
	<folder>language</folder>
</files>
<languages>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.ini</language>
	<language tag="en-GB">language/en-GB/en-GB.plg_system_languagecode.sys.ini</language>
</languages>
```

The advantages of this solution are the following:

All *.ini* files present in the core folder have precedence over the
files in the extension *language/* folder. For example, a *.sys.ini*
file will always be loaded from core folders in the back-end if it
exists, except when installing an extension which contains a *.sys.ini*
file in a language folder. In that case and only that case, the
*.sys.ini* file in the extension folder will display its translated
content at install time. This is very handy. As a developer can have two
*.sys.ini* files with different contents.

Also, it is much easier for a user needing an *.ini* file for an
extension that does not provide it in the language desired to add it in
the main folders. There is no risk that it will be deleted in case of
uninstalling the extension by mistake or any other reason.

See also:

- [Making non-core language
  packs](https://docs.joomla.org/J2.5:Making_non-core_language_packs "Special:MyLanguage/J2.5:Making non-core language packs")
- [Creating language packs for extensions in Joomla
  2.5](https://docs.joomla.org/Creating_language_packs_for_extensions_in_Joomla_2.5 "Special:MyLanguage/Creating language packs for extensions in Joomla 2.5")

During development you can turn on language debugging in the Joomla!
global configuration. You can investigate if a problem arises. As of
3.2, this is necessary to help debug as en-GB is **always** loaded first
when not in debug mode to prevent displaying Constants.

### Script file

```xml
    <scriptfile>example.script.php</scriptfile>
```

An optional **script file** (PHP code that is run before, during and/or
after installation, uninstallation and upgrading) can be defined using a
element.

This file should contain a class named "InstallerScript" where is the
name of your extension (e.g. com_componentname, mod_modulename, etc.).
Plugins must state the group (e.g. plgsystempluginname).

In <img src="https://docs.joomla.org/images/f/f4/Compat_icon_4_0.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 4.0" /> and later the structure of the class is
as follows:

```php
<?php

use Joomla\CMS\Installer\InstallerAdapter;

class com_componentnameInstallerScript
{
	/**
	 * Constructor
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function __construct(InstallerAdapter $adapter)
	{
	}
	
	/**
	 * Called before any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function preflight($route, InstallerAdapter $adapter)
	{
		return true;
	}
	
	/**
	 * Called after any type of action
	 *
	 * @param   string  $route  Which action is happening (install|uninstall|discover_install|update)
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function postflight($route, $adapter)
	{
		return true;
	}
	
	/**
	 * Called on installation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function install(InstallerAdapter $adapter)
	{
		return true;
	}
	
	/**
	 * Called on update
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 *
	 * @return  boolean  True on success
	 */
	public function update(InstallerAdapter $adapter)
	{
		return true;
	}
	
	/**
	 * Called on uninstallation
	 *
	 * @param   InstallerAdapter  $adapter  The object responsible for running this script
	 */
	public function uninstall(InstallerAdapter $adapter)
	{
		return true;
	}
}

?>
```

Note that since Joomla 3.6 Joomla has shipped a basic script that you
can use instead of shipping your own from scratch **JInstallerScript**
which contains various helper methods commonly used through the
community.

### Library Manifests

A simple library manifest might look like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="library" method="upgrade" version="4.0">
    <name>My Test library.</name>
    <libraryname>mytest</libraryname>
    <files>
        <folder>Classes</folder>
        <folder>language</folder>
        <filename>mytest.php</filename>
    </files>
</extension>
```

This will install the library into the `JPATH_SITE/libraries/mytest`
folder.

What if your company has several libraries and you want to group them
together under folder `JPATH_SITE/libraries/mycompany`?

Simple - include your company name in the `libraryname` property of each
library like this:

```xml
    <libraryname>mycompany/mylibrary1</libraryname>
```

```xml
    <libraryname>mycompany/mylibrary2</libraryname>
```

These libraries will then be installed in the
`JPATH_SITE/libraries/mycompany/mylibrary1` and
`JPATH_SITE/libraries/mycompany/mylibrary2` folders.

Uninstalling `mylibrary1` will still leave `mylibrary2` installed on
your site.

When using `script files` with such company libraries the installer
class name should look like this:

```php
    class mycompanymylibrary1InstallerScript
```

```php
    class mycompanymylibrary2InstallerScript
```

### Update Servers

```xml
    <updateservers>
        <server type="extension" priority="1" name="Extension Update Site">http://example.com/extension.xml</server>
        <server type="collection" priority="2" name="Collection Update Site">http://example.com/collection.xml</server>
    </updateservers>
```

Update servers can be defined in the element, a child of the root. This
element may contain one or more element, each describing the location
from which to download updates. Each item can define the following
attributes:

<table class="wikitable">

<tbody>
<tr class="header">
<th>Attribute</th>
<th>Values</th>
<th>Description</th>
</tr>
&#10;<tr class="odd">
<td>type</td>
<td><code>extension</code><br />
<code>collection</code></td>
<td>The update server type</td>
</tr>
<tr class="even">
<td>priority</td>
<td><em>integer</em></td>
<td>The priority of the update server</td>
</tr>
<tr class="odd">
<td>name</td>
<td><em>string</em></td>
<td>The name of the update server</td>
</tr>
</tbody>
</table>  

More info:

- [Building a Joomla! Extension - Adding an update
  server](https://docs.joomla.org/J3.x:Developing_an_MVC_Component/Adding_an_update_server "Special:MyLanguage/J3.x:Developing an MVC Component/Adding an update server")
- [Managing Component Updates in Joomla
  2.5](https://docs.joomla.org/J2.5:Managing_Component_Updates "Special:MyLanguage/J2.5:Managing Component Updates")

### Supporting Download Keys

As of Joomla 4.0.0 users can enter their download keys in the Update
Sites list. This gives them a single place to manage the download keys.
When a user is going to update an extension, Joomla will check if there
is a download key. If there is a download key, Joomla will add the
download key to the update URL.

To support download keys you must include the `dlid` tag in the manifest
file. The `dlid` tag takes 2 arguments:

- prefix
- suffix

The `dlid` tag will look like this in your manifest file:

```xml
<dlid prefix="dlid=" suffix="&amp;dummy=my.zip"/>
```

The prefix will be added before the download key and the suffix after
the download key. Using the example above the full query added to the
download link will be:

    dlid=KEY&dummy=my.zip

The key is added before the `onInstallerBeforePackageDownload` event is
triggered, so the full URL will be passed to the event.

## Examples

For a real-life example, see <a
href="https://github.com/joomla/joomla-cms/blob/3.9.16/administrator/components/com_banners/banners.xml"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">the manifest of the Banner component
in the latest version of Joomla! 3.9.16</a>.

Some more examples can be found in the standalone weblinks repo:

- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/administrator/components/com_weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">com_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/modules/mod_weblinks/mod_weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">mod_weblinks manifest</a>
- <a
  href="https://github.com/joomla-extensions/weblinks/blob/master/src/plugins/search/weblinks/weblinks.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">plg_search_weblinks manifest</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.9.16/templates/protostar/templateDetails.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">tpl_protostar manifest (3.9.16)</a>
- <a
  href="https://github.com/joomla/joomla-cms/blob/3.9.16/administrator/language/en-GB/en-GB.xml"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">en-GB manifest (3.9.16)</a>
