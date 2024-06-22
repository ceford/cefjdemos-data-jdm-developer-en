<!-- Filename: Adding_changelog_to_your_manifest_file / Display title: Adding a Changelog -->

Since Joomla 4.0, extension developers can leverage the ability of Joomla to read a changelog file and give a visual representation of the changelog. If a given version is not found in the changelog, the changelog button will not be shown.

The changes in a release will be presented in this manner:

<img alt="Changelog modal" src="https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/700px-Changelog_modal-en.png" decoding="async" width="700" height="461" srcset="https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/1050px-Changelog_modal-en.png 1.5x, https://docs.joomla.org/images/thumb/7/7a/Changelog_modal-en.png/1400px-Changelog_modal-en.png 2x" data-file-width="1618" data-file-height="1066">

The changelog is used in two different places.

## Update View

The installer will show the changelog of the version that can be installed if available.

<img alt="Changelog button on the Update View" src="https://docs.joomla.org/images/thumb/7/79/Update_view_changelog_button-en.png/700px-Update_view_changelog_button-en.png" decoding="async" width="700" height="152" srcset="https://docs.joomla.org/images/thumb/7/79/Update_view_changelog_button-en.png/1050px-Update_view_changelog_button-en.png 1.5x, https://docs.joomla.org/images/7/79/Update_view_changelog_button-en.png 2x" data-file-width="1282" data-file-height="278">

Clicking the Changelog button here will show the changelog of the new available version.

## Manage View

The extension manager will show the changelog of the currently installed extension if available.

<img alt="Version number is a link to the changelog modal" src="https://docs.joomla.org/images/thumb/4/4b/Manage_view_changelog_link-en.png/700px-Manage_view_changelog_link-en.png" decoding="async" width="700" height="81" srcset="https://docs.joomla.org/images/thumb/4/4b/Manage_view_changelog_link-en.png/1050px-Manage_view_changelog_link-en.png 1.5x, https://docs.joomla.org/images/4/4b/Manage_view_changelog_link-en.png 2x" data-file-width="1274" data-file-height="148">

Clicking the version number here will show the changelog of the current installed version.

## Add changelogurl tag to manifest files

The first step is to update your manifest files that tell Joomla where to find the changelog details. Add the following node to your manifest XML files:

```
<changelogurl>https://example.com/updates/changelog.xml</changelogurl>
```

Please note: The URL in the changelogurl tag must not have any spaces or line breaks before or after it. See code examples.

### Update server manifest

See this example for an update server manifest file that informs Joomla about an update of a component named "com_lists". Thus you will see the Changelog button in the update view.

```
<?xml version="1.0" encoding="utf-8"?>
<updates>
 <update>
  <name>Student List</name>
  <description>List of students</description>
  <element>com_lists</element>
  <type>component</type>
  <version>4.0.0</version>

  <changelogurl>https://example.com/updates/changelog.xml</changelogurl>

  <tags>
   <tag>stable</tag>
  </tags>
  <maintainer>Example Miller</maintainer>
  <maintainerurl>https://example.com/</maintainerurl>
  <section>Updates</section>
  <targetplatform name="joomla" version="4.?" />
  <client>1</client>
  <folder></folder>
 </update>
</updates>
```

### Extension manifest

Additionally add the changelogurl tag to the extension manifest XML. Thus the extension version will be linked to the changelogs in the manage view.

```
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" method="upgrade">
	<name>COM_LISTS</name>

... Other stuff ...

	<changelogurl>https://example.com/updates/changelog.xml</changelogurl>

	<updateservers>
        <server type="extension" name="My Extension's Updates">https://example.com/lists-updates.xml</server>
	</updateservers>
</extension>
```
## Create changelog file

The changelog file must have the following 3 nodes:

* element
* type
* version

This information is used to identify the correct changelog for a given extension.

A version node inside any changelog node is always mandatory. Otherwise you will see an error message like SyntaxError: JSON.parse: unexpected character at line 1 column 1 of the JSON data.

```
<element>com_lists</element>
<type>component</type>
<version>4.0.0</version>
```

Further the changelog is filled with one or more change types. The following change types are supported:

* security: Any security issues that have been fixed
* fix: Any bugs that have been fixed
* language: This is for language changes
* addition: Any new features added
* change: Any changes
* remove: Any features removed
* note: Any extra information to inform the user

Each node can be repeated as many times as needed.

The format of the text can be plain text or HTML but in case of HTML, it must be enclosed in CDATA tags as shown in the example.

```
<changelogs>
    <changelog>
        <element>com_lists</element>
        <type>component</type>
        <version>4.0.0</version>
        <security>
            <item>Item A</item>
            <item><![CDATA[<h2>You MUST replace this file</h2>]]></item>
        </security>
        <fix>
            <item>Item A</item>
            <item>Item b</item>
        </fix>
        <language>
            <item>Item A</item>
            <item>Item b</item>
        </language>
        <addition>
            <item>Item A</item>
            <item>Item b</item>
        </addition>
        <change>
            <item>Item A</item>
            <item>Item b</item>
        </change>
        <remove>
            <item>Item A</item>
            <item>Item b</item>
        </remove>
        <note>
            <item>Item A</item>
            <item>Item b</item>
        </note>
</changelog>
<changelog>
	<element>com_lists</element>
	<type>component</type>
	<version>0.0.2</version>
	<security>
		<item>Big issue</item>
	</security>
</changelog>
</changelogs>
```

This file contains 2 changelogs:

* Version 0.0.2 (for testing the manage view)
* Version 4.0.0 (for testing the update view)

A changelog can have as many versions as needed
