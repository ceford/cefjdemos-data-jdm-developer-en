<!-- Filename: Creating_a_Smart_Search_plug-in / Display title: Example: Smart Search -->

## Introduction

The Smart Search plugins are located in the `finder` group (in plugins/finder/xxxx). The coding of finder plugins has changed significantly in Joomla 4 and 5. To create a new finder plugin it is probably best to copy an existing core plugin and modify its content to suit its new purpose.

This article describes a finder plugin designed to support the *Jdocmanual* extension. The content to be indexed is located in the table `#__jdm_articles`. The code can be found on [GitHub](https://github.com/ceford/cefjdemos-plg-finder-jdocmanual).

## IDE Setup - VSCode or VSCodium

In my local development environment, the source code of this extension is located in ~/git/cefjdemos-plg-jdm-finder. This folder name has no significance; it is just my way of keeping my own extensions in order. The schematic structure of the folder is as follows:

```
cefjdemos-plg-finder-jdocmanual
|-- .vscode (folder for build settings)
|-- plg_finder_jdocmanual (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_finder_jdocmanual.ini
        |-- plg_finder_jdocmanual.sys.ini
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Jdocmanual.php (the extension class code)
    |-- jdocmanual.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

In VSCodium it looks like this:

![Plugin development file structure in vscodium](../../../en/images/plugins/jdocmanual-vscodium.png)

## Customise the Code

Having started with a core finder plugin the first task is to change all of the references to the original plugin to suitable values for the new plugin. In this case there are 63 instances of the word `jdocmanual` in 7 files. There is a good chance that some will be missed on the first pass and the plugin will not work.

## Build the Extension

There are two parts to the build. I have a build.xml file that contains instructions for [`phing`](https://www.phing.info/), a build tool for PHP projects. It can be called from VSCode/VSCodium or Eclipse or any other IDE.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="jdocmanual" basedir="." default="main">

    <property name="joomladir" value="/Users/ceford/public_html/jdm3"  override="true" />
    <property name="sitecomdir" value="${joomladir}/components" override="true" />
    <property name="admincomdir" value="${joomladir}/administrator/components" override="true" />
    <property name="adminmediadir" value="${joomladir}/media/com_jdocmanual" override="true" />
    <property name="plgdir" value="${joomladir}/plugins/finder/jdocmanual" override="true" />
    <property name="srcdir" value="${project.basedir}" override="true" />

    <property name="zipsdir" value="/Users/ceford/git/zips/cefjdemos"  override="true" />

    <!-- Fileset for plugin files -->
	<fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
	</fileset>

	<!-- fileset for zip -->
	<fileset dir="./plg_finder_jdocmanual" id="plgfiles">
        <include name="**" />
	</fileset>

    <!-- ============================================  -->
    <!-- (DEFAULT) Target: main                        -->
    <!-- ============================================  -->
    <target name="main" description="main target">

        <copy todir="${plgdir}">
            <fileset refid="plgfiles" />
        </copy>

        <zip destfile="${zipsdir}/plg_finder_jdocmanual.zip">
            <fileset refid="plgfiles" />
        </zip>

    </target>
</project>
```

The second part is a `tasks.json` file in the .vscode folder. It tells VSCode where to find the phing file.

```json
{
	// See https://go.microsoft.com/fwlink/?LinkId=733558
	// for the documentation about the tasks.json format
	"version": "2.0.0",
	"tasks": [
	  {
		"label": "Build plg_finder_jdocmanual",
		"type": "shell",
		"command": "php ~/bin/phing-latest.phar",
		"windows": {
		  "command": "php ~/bin/phing-latest.phar"
		},
		"group": "build",
		"presentation": {
		  "reveal": "always",
		  "panel": "shared"
		}
	  }
	]
}
```

In VSCode/VScodium I select **Terminal / Run Build Task...** from the menu bar and select the appropriate task.

## Install the Extension

After the build, the extension zip file must be installed in the same way as any other extension. It needs to be enabled and any options need to be set. Jdocmanual has three *Taxonomies to Index* options:

- **Manual** one or all of the available Manuals (User, Developer, ...)
- **Language** searches are restricted to articles in the same language as the page language.
- **Type** one or all of the types of content (Jdocmanual, Content, ...)

In the case of the Jdocmanual site, other finder plugins are disabled because there is no other content to be indexed.

After the first installation it is normally sufficient to run the build task again after code correction. Installation of the zip file is only required if there are changes to the manifest.xml file.

## Index the Content

Jdocmanual is setup to index its articles when requested by a Super User. This is unlike the normal Content plugin which indexes content whenever an article is saved. A first run takes a few minutes to index the ~5000 Jdocmanual articles in 8 languages.

## Create a Smart Search Module

From the **Content / Site Modules** select **New** and install a new **Smart Search** module. Adjust the settings to obtain a suitable appearance.

## Testing

Eventually, your custom smart search plugin should work. This is an example results page for Jdocmanual that searches for a term in this page. The result page omits the search form from the title bar because it is present in the page body.

![Smart search result](../../../en/images/plugins/jdocmanual-search-result.png)

An aside: the System - Joomla Accessibility Checker plugin shows there are 3 errors related to the *Search Terms* data entry form. That needs a core fix or override.
