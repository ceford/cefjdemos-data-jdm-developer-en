<!-- Filename: Deploying_an_Update_Server / Display title: Deploying an Update Server -->

Joomla!  2.5 Joomla!  3.x Joomla!  4.x

## Introduction

This tutorial is designed to teach developers how to create an update
server for integration with the update system introduced in Joomla!. By
adding an update server listing to your extension's manifest, developers
enable users to update their extensions via the Extension Manager's
Update view with only a few clicks.

## Defining an update server

In order to use this feature, an update server must be defined in your
extension's manifest. This definition can be used in all Joomla! 2.5 and
newer compatible extensions but is not available for templates. You can
use two options for your server type: collection or extension. These
will be explained in detail shortly. This code should be added to the
extension manifest file, within the root *extension* element. The update
server is defined as follows for each type:

```xml
<extension>
<...>
<updateservers>
  <server type="collection">https://example.com/list.xml</server>
  <server type="extension" priority="2" name="My Extension's Updates">http://example.com/extension.xml</server>
</updateservers>
</extension>
 ```

Multiple servers can be defined within the `<updateservers>` tag. If you have more than
one update server, you can set a different priority for each. In that
way you can control the order in which the update servers are checked.

## Server types

### Collection

The `"collection"` server type allows developers to define an
extension's manifest to pull updates from a collection. This type of
server can be used if the developer wants to define all of their
extension's updates in a single file (not recommended) or if their
extension has multiple sub-extensions which are not distributed or
updated at the same time (such as a package extension type). The below
example is the `"collection"` definition used by the updater when
processing core Joomla! updates:

```xml
<extensionset name="Joomla Core" description="Joomla! Core">
    <extension name="Joomla" element="joomla" type="file" version="1.7.0" detailsurl="https://update.joomla.org/core/extension.xml"/>
</extensionset>
```

All definitions must be defined between `<extensionset>` tags in your collection
manifest. The `<extensionset>` tag has two optional parameters; name and description. For
each extension that this collection references, a separate `<extension>` tag is
required. The `<extension>` tag has the following parameters, all of which are
required for updates to properly process:

- **name** – The name of the extension
- **element** – The untranslated extension name i.e. mod_custom
- **type** – The extension type (component, module, plugin, etc.)
- **version** – The latest version of the extension
- **detailsurl** – The URL of the XML file which contains that
  extension's individual update definitions

### Extension

The `"extension"` server type allows developers to define an extension's
manifest to pull updates from a single extension's manifest. All
collection manifests eventually point to this XML file. All updates in
this file must be defined after an `<updates>` tag at the beginning of the file.
The below example is the update definition for the Joomla! 3.9.6 release:

```xml
<updates>
	<update>
		<name>Joomla! 3.9</name>
		<description>Joomla! 3.9 CMS</description>
		<element>joomla</element>
		<type>file</type>
		<version>3.9.6</version>
		<infourl title="Joomla!">https://www.joomla.org/announcements/release-news/5765-joomla-3-9-6-release.html</infourl>
		<downloads>
			<downloadurl type="full" format="zip">https://downloads.joomla.org/cms/joomla3/3-9-6/Joomla_3.9.6-Stable-Update_Package.zip</downloadurl>
			<downloadsource type="full" format="zip">https://github.com/joomla/joomla-cms/releases/download/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
			<downloadsource type="full" format="zip">https://update.joomla.org/releases/3.9.6/Joomla_3.9.6-Stable-Update_Package.zip</downloadsource>
		</downloads>
		<tags>
			<tag>stable</tag>
		</tags>
		<sha256>05157273aadd3045564ee44373ea3643b437fa5321d17993a3119b38b04578e2</sha256>
		<sha384>ebd9b0666fbe84e20a420a4bcd6c10d306fc4dee4edbbe8e2133c85f0fb84e59be5a50aa97cb38c068b77f77f6bbc091</sha384>
		<sha512>a4c47644ceeaeec28944e0c74160203cf12037e0ea1439022e95055dfb6716de172667ce6d9164f12bb519d9cfcf1fdc728abea00f853b41debc7d2740f2b711</sha512>
		<maintainer>Joomla! Production Department</maintainer>
		<maintainerurl>https://www.joomla.org</maintainerurl>
		<section>STS</section>
		<targetplatform name="joomla" version="3.[789]" />
		<php_minimum>5.3.10</php_minimum>
</update>
</updates>
```

The following section describes the elements of a single update entity.

- **name** – The name of the extension, this name will appear in the
  Name column of the Extension Manager's Update view (required)
- **description** – A short description of the extension (optional) — if
  you choose to use `<![CDATA[]]>`, double-quotes will break the HTML formatting. Use
  single quotes with your HTML entities.
- **element** – The installed name of the extension (required). For
  plugins, this needs to be same as plugin attribute value for main file
  in plugin manifest. For `<filename plugin="pluginname">pluginname.php</filename>`
  pluginname.php, element value should be **pluginname**.
- **type** – The type of extension (component, module, plugin, etc.)
  (required)
- **folder** – Specific to plugins, this tag describes the type of
  plugin being updated (content, system, etc.) (required for plugins)
- **client** – The client of the extension. Required for modules and
  templates as of 3.2.0. Possible values at this time are "site" or
  "administrator". **Warning!** Plugins and front-end modules are
  automatically installed with a client of site, but you will need to
  specify the client in an update or it will default to administrator
  and then found update would not be shown because it would not match
  any extension. Components are automatically installed with a client of
  administrator, which is currently the default.
  - **Warning**: As of Joomla! 4.0 only a string is allowed. Using
    numbers as client has been deprecated in 2012 and got removed in
    Joomla! 4.0 (Replace 0 with "site" and 1 with "administrator")
  - **Warning**: The tag name is \<**client**\> for Joomla! 2.5 and
    \<**client_id**\> for 1.6 and 1.7. If you use (rather than ) on a
    2.5 site, it will be ignored.
- **version** – The version of the release (required)
- **infourl** – A URL to point users to containing information about the
  update (optional) (In CMS 2.5, if set, this URL will be displayed in
  the update view)
- **downloads** – The section which lists all download locations
  - **downloadurl** – The URL to download the extension from; the `<downloadurl>`
  tag has two required parameters:
    - **type** – The type of package (full or upgrade)
    - **format** – The format of the package (zip, tar, etc.)
  - **downloadsource** – Optional. Since Joomla 3.8.3. Alternative URL
    to download the extension from when the connection to fails.
    Multiple `<downloadsource>` tags are allowed. The `<downloadsource>` tag has two required parameters:
    - **type** – The type of package (full or upgrade)
    - **format** – The format of the package (zip, tar, etc.)
  - **NB** – there must be no newline before or after the URL; it needs
    to all be on one line or you will get Error connecting to the
    server: malformed when the update is run
- **changelogurl** - A link to an xml file holding the changelog. Joomla
  4.0 and later will allow you to show a button for the changelog in the
  extension updater page. More details in how to use it can be found
  here: [Adding changelog to your manifest
  file](https://docs.joomla.org/Adding_changelog_to_your_manifest_file "Adding changelog to your manifest file")
- **tags** – A list of tags relevant to this version. Joomla! 3.4 and
  later uses this to determine the stability level of the update. The
  valid tags are:
  - *dev*: Development versions, very unstable and pre-alpha (e.g.
    nightly builds)
  - *alpha*: Alpha quality software (features not implemented,
    show-stopper bugs)
  - *beta*: Beta quality software (all features implemented,
    show-stopper bugs possible, minor bugs almost certain)
  - *rc*: Release Candidate quality software (no show-stopper bugs,
    minor bugs may still be present)
  - *stable*: Production quality software All other tags are currently
    ignored. If you provide more than one tag containing one of the
    aforementioned stability keywords only the LAST tag will be taken
    into account. If you do not provide any tags Joomla! will assume it
    is a stable version.
- **maintainer** – The name of the extension maintainer (similar to the
  `<author>` tag in a manifest) (optional)
- **maintainerurl** – The website of the extension maintainer (similar
  to the `<authorUrl>` tag in a manifest) (optional)
- **section** – Optional (unknown use)
- **targetplatform** – A tag to define platform requirements (from
  <img src="https://docs.joomla.org/images/5/55/Compat_icon_3_10.png"
  decoding="async" data-file-width="40" data-file-height="17" width="40"
  height="17" alt="Joomla 3.10" /> this is also used to detect extension
  compatibility for the Joomla Update component), requires the following
  elementsː
  - **name** – The name of the platform dependency; as of this writing,
    it should ONLY be "joomla"
  - **version** – The version of Joomla! the extension supports
  - **min_dev_level** and **max_dev_level** – These attributes were
    added in 3.0.1 to allow you to select a target platform based on the
    developer level ("z" in x.y.z). They are optional. You can specify
    either one or both. If omitted, all developer levels are matched.
    For example, the following matches versions 4.0.0 and 4.0.1.
    `<targetplatform name="joomla" version="4.0" min_dev_level="0" max_dev_level="1"/>`
    - **Note:** If your extension is Joomla! 2.5 and/or 3.1 compatible,
      you will be required to have separate `<update>` definitions for each version
      due to the manner in which the updater checks the version if you
      specify a number. However to show your extension on all Joomla
      versions that support automatic updates (and thus mark as
      compatible with all future unreleased versions of Joomla in Joomla
      Update) add `<targetplatform name="joomla" version=".*"/>`. If you want your extension to show on all
      <img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
      decoding="async" data-file-width="40" data-file-height="17" width="40"
      height="17" alt="Joomla 3.x" /> versions then rather than
      specifying a version in the version tag add in `<targetplatform name="joomla" version="3.[012345]"/>`. This will show
      the update to all 3.x versions from version 3.0 to 3.5. If you
      want to include version 3.10 you can use an `|` like this: `<targetplatform name="joomla" version="3.[012345]|10"/>`. If
      you want to show the updates for all 3.8.x versions and all 3.10.x
      versions you can use `<targetplatform name="joomla" version="3.(8|10)"/>`.
- **php_minimum** – Beginning with 3.2.2, a minimum supported PHP
  version can be supplied in the update stream. If the server does not
  meet the minimum, a message is displayed to the user advising that an
  update is available but cannot be installed due to unsupported
  requirements.
- **supported_databases** – Beginning with 3.7, a minimum supported
  databases + version check can be supplied in the update stream. When
  the server does not meet the minimum, a message is displayed to the
  user advising that an update is available but cannot be installed due
  to unsupported requirements. Note:
  <a href="https://github.com/joomla/joomla-cms/pull/26079"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">As of 3.9.12</a> there is also a
  special case for mariadb so you can set a specific minimum version for
  that too now.
  - An example could look like this: `<supported_databases mysql="5.5.3" mariadb="10.1" postgresql="9.2" mssql="10.50.1600.1" />`
- **sha256**, **sha384**, **sha512** – Optional. Since Joomla 3.9.0 you
  can add file checksums in these hash formats. Be aware that in Joomla
  3 only a notice is shown on **updates** if a checksum is not correct.
  That's all. The update doesn't stop then. In Joomla 4 updates and
  installations will stop if a provided checksum doesn't match.

A separate `<update>` definition will be required for each version of your
extension you release.

The values of **element**, **type**, **client_id** and **folder** should
match those in the table \#\_extensions.

**Important for plugins:** Plugins have to include `<folder>` and `<cient>`
elements to work properly

## Troubleshooting

- **SQL update script is not executed during update.**

If the SQL update script (for example, in the folder
`sql/updates/mysql`) does not get executed during the update process, it
could be because there is no version number in the `#_schemas` table for
this extension *prior to the update*. This value is determined by the
last script name in the SQL updates folder. If this value is blank, no
SQL scripts will be executed during that update cycle. To make sure this
value is set correctly, make sure you have a SQL script in this folder
with its name as the version number (for example, 1.2.3.sql if the
version is 1.2.3). The file can be empty or just have a SQL comment
line. This should be done in the old version — the one before the
update. Alternatively, you can add this value to the `#_schemas` using a
SQL query.

## Supporting Tools
