<!-- Filename: J4.x:Developer:_Required_Software / Display title: Language Extension Example -->

## Introduction

Official Joomla language extensions are normally installed via the System &rarr; Install &rarr; Languages route. However, there may be occasions when it is necessary to install a language extension via the Install &rarr; Extensions &rarr; Upload & Install route. This example is for Scottish Gaelic with all of the English to Gaelic translation obtained using openai.com at a cost of just under $5. It is an unofficial language extension because it really needs the translations verified by Gaelic speakers, of which there are about 60,000 in total. Creation of the extension was inspired by the coincidence of an enquiry in the Forum and a personal visit to the ruins of Carnasserie Castle where the very first printed document in Scottish Gaelic was produced in 1567.

## Local Repository File Structure

The following local repository structure is the same as that present in the remote GitHub [repository](https://github.com/ceford/cefjdemos-pkg-gd-gb). The .ini files are translations of the original English .ini files. The method of translation is covered in a separate [article](jdocmanual?article=developer/languages/translate-openai). The downloads folder contents are created by the GitHub workflow and downloaded to the local repository with `git pull`.

```sh
cefjdemos-pkg-gd-gb
    .github
        workflows
            package.yml
    downloads
        pkg_gd-GB-joomla5.zip
        pkg_gd-GB-joomla5.zip.sha256
        pkg_gd-GB-joomla5.zip.sha384
        pkg_gd-GB-joomla5.zip.sha512
    gd-GB
        admin_gd-GB
            454 *.ini files
            install.xml
            langmetadata.xml
            localise.php
        api_gd-GB
            2 *.ini files
            install.xml
            langmetadata.xml
        site_gd-GB
            69 *.ini files
            install.xml
            langmetadata.xml
            localise.php
    pkg_gd-GB.xml
    .gitignore
    LICENSE
    README.md
    updates.xml
```
The local repository has two branches: joomla5 and joomla6. This article does not cover the joomla6 version, which is almost identical except for version numbers and the actual contents of the ini files. There are more entries in the ini files in Joomla 6.

## The pkg_gd-GB.xml File

Note that **gd-GB** is the ISO code for Scottish Gaelic. Most of the fields in the pkg_gd-GB.xml file are self-explanatory. It is possible to create separate Site and Administrator language extensions. However, the Administrator install.xml and langmetadata.xml files are required for language administration and the localise.php file is required for use by some plugins.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="package" method="upgrade">
	<name>Scottish Gaelic</name>
	<packagename>gd-GB</packagename>
	<version>5.3.1.2</version>
	<creationDate>2025-06-18</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@ford.myzen.co.uk</authorEmail>
	<authorUrl>https://github.com/ceford/cefjdemos-pkg-gd-gb</authorUrl>
	<copyright>(C) 2025 Clifford E Ford. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<url>https://github.com/ceford/cefjdemos-pkg-gd-gb</url>
	<packager>Clifford E Ford</packager>
	<packagerurl>https://github.com/ceford/cefjdemos-pkg-gd-gb</packagerurl>
	<description><![CDATA[Scottish Gaelic translation created by openai.com]]></description>
	<blockChildUninstall>true</blockChildUninstall>
	<files>
		<file type="language" client="site" id="gd-GB">site_gd-GB.zip</file>
		<file type="language" client="administrator" id="gd-GB">admin_gd-GB.zip</file>
		<file type="language" client="api" id="gd-GB">api_gd-GB.zip</file>
	</files>
	<updateservers>
		<server type="extension" priority="2" name="Scottish Gaelic Update Site">https://github.com/ceford/cefjdemos-pkg-gd-gb/raw/main/updates.xml</server>
	</updateservers>
</extension>
```

The extension version is usually the same as the Joomla version for which it was created. An optional extra parameter may be used for updates, for example 5.3.1.1. When creating a third party extension take care not to copy any Official Joomla! elements. The JED Checker will flag some as invalid.

**Important:** A version number is maintained in this file for update purposes but all versions of Joomla 5 (or 6) will use the latest language version.

## Administrator

The admin folder contains a large number of individual `.ini` files and three others: `install.xml`, `langmetadata.xml` and `localise.php`.

### install.xml

This file is used for installation and removal of the language extension.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension client="administrator" type="language" method="upgrade">
	<name>Scottish Gaelic</name>
	<tag>gd-GB</tag>
	<version>5.3.1.2</version>
	<creationDate>2025-06-18</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@ford.myzen.co.uk</authorEmail>
	<authorUrl>https://github.com/ceford/cefjdemos-pkg-gd-gb</authorUrl>
	<copyright>(C) 2025 Clifford E Ford. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<description><![CDATA[Scottish Gaelic translation created by openai.com]]></description>

	<files>
		<folder>/</folder>
		<filename>langmetadata.xml</filename>
		<filename>install.xml</filename>
	</files>
</extension>
```

### langmetadata.xml

This file is used for language management purposes.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metafile client="administrator">
	<name>Scottish Gaelic</name>
	<tag>gd-GB</tag>
	<version>5.3.1.1</version>
	<creationDate>2025-06-18</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@ford.myzen.co.uk</authorEmail>
	<authorUrl>https://github.com/ceford/cefjdemos-pkg-gd-gb</authorUrl>
	<copyright>(C) 2025 Clifford E Ford. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<description><![CDATA[Scottish Gaelic translation created by openai.com]]></description>
	<metadata>
		<name>Scottish Gaelic</name>
		<nativeName>Gàidhlig na h-Alba</nativeName>
		<tag>gd-GB</tag>
		<rtl>0</rtl>
		<locale>gd_GB.utf8, gd_GB.UTF-8, gd_GB, gd, gla, gd-GB, scottish gaelic, gaelic, scots gaelic, scotland, uk, united kingdom</locale>
		<firstDay>1</firstDay>
		<weekEnd>0,6</weekEnd>
		<calendar>gregorian</calendar>
	</metadata>
	<params />
</metafile>
```

#### Notes

- The `<name>` tag should be in English.
- The `<nativeName>` tag should be in the extension language.
- The `<locale>` tag is used for sorting purposes. It should include:
    - Standard POSIX-style locale codes (e.g., gd_GB.utf8)
    - Alternate capitalizations or encodings (gd_GB.UTF-8, gd_GB)
    - ISO language and country codes (gd, gla, gd-GB)
    - Human-readable names and aliases (scottish gaelic, scots gaelic, gaelic, etc.)
    - Country/region-related terms (scotland, uk if applicable)
- The `<firstDay>` tag is used to specify the first day of the week in that language. 0 is Sunday, 1 is Monday, etc.
- The `<weekEnd>` tag is used to define the days considered to be weekend and often greyed. 0,6 is Saturday & Sunday, 1 would be Friday.
- The `<calendar>` tag uses *gregorian* by default. Other calendars may be available for some languages.

### localise.php

This file is used to cope with language peculiarities.

```php
<?php
/**
 * @package    Joomla.Language
 *
 * @copyright (C) 2025 Clifford E Ford. All rights reserved.
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 *
 * @phpcs:disable Squiz.Classes.ValidClassName.NotCamelCaps
 *
 * @phpcs:disable PSR1.Classes.ClassDeclaration.MissingNamespace
 */

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * gd-GB localise class.
 *
 * @since  1.6
 */
abstract class Gd_GBLocalise
{
    /**
     * Returns the potential suffixes for a specific number of items
     *
     * @param int $count  The number of items.
     *
     * @return  array  An array of potential suffixes.
     *
     * @since   1.6
     */
    public static function getPluralSuffixes($count)
    {
        if ($count == 0) {
            return ['0'];
        } elseif ($count == 1) {
            return ['ONE', '1'];
        } else {
            return ['OTHER', 'MORE'];
        }
    }

    /**
     * Returns the ignored search words
     *
     * @return  array  An array of ignored search words.
     *
     * @since   1.6
     *
     * @deprecated  5.1 will be removed in 7.0 without replacement
     */
    public static function getIgnoredSearchWords()
    {
        return ['agus', 'ann', 'air', 'an', 'am', 'aig', 'le', 'do', 'gu'];
    }

    /**
     * Returns the lower length limit of search words
     *
     * @return  integer  The lower length limit of search words.
     *
     * @since   1.6
     *
     * @deprecated  5.1 will be removed in 7.0 without replacement
     */
    public static function getLowerLimitSearchWord()
    {
        return 3;
    }

    /**
     * Returns the upper length limit of search words
     *
     * @return  integer  The upper length limit of search words.
     *
     * @since   1.6
     *
     * @deprecated  5.1 will be removed in 7.0 without replacement
     */
    public static function getUpperLimitSearchWord()
    {
        return 20;
    }

    /**
     * Returns the number of chars to display when searching
     *
     * @return  int  The number of chars to display when searching.
     *
     * @since   1.6
     *
     * @deprecated  5.1 will be removed in 7.0 without replacement
     *
     */
    public static function getSearchDisplayedCharactersNumber()
    {
        return 200;
    }
}
```

## API and Site

The files in these folders are similar to those in the admin folder except that the client attribute is set to *api* and *site* respectively and there are fewer *.ini files. The files are not reproduced here. See the en-GB versions for examples.

## The GitHub Workflow

Package creation involves several steps:

- Create individual zip files for each client (admin,api and site).
- Create final package zip.
- Generate SHA hashes.
- Save package and hashes for public access.
- Update the `updates.xml` file with current metadata.

This work is accomplished on GitHub with the .github/workflows/package.yml file. It is triggered every time there is a commit to the repository. 

### The package.yml file

Some preliminary notes:

- workflow_dispatch: is used for manually triggering a run using a GitHub button.
- permissions: contents: write is needed to allow the workflow to save its output.
- env: EXTENSION_NAME: pkg_gd-GB is the item to change for a different extension.
- steps: are the individual steps required to build the extension.
- the last two steps update and save the `updates.xml` file.

```yml
name: Build Joomla Language Extension Package

on:

  push:
    branches:
      - joomla5
      - joomla6

  workflow_dispatch:
    inputs:
      version:
        description: 'Custom version (optional, overrides pkg_gd-GB.xml <version>)'
        required: false
      forceRebuild:
        description: 'Force rebuild all zip files (true/false)'
        required: false
        default: 'false'

permissions:
  contents: write

env:
  EXTENSION_NAME: pkg_gd-GB
  LANGUAGE_CODE: gd-GB
  LC_LANGUAGE_CODE: gd-gb

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Install xmllint
        run: sudo apt-get update && sudo apt-get install -y libxml2-utils

      - name: Set version and target
        id: vars
        run: |
          BRANCH_NAME="${GITHUB_REF##*/}"

          # Use manual version input if provided, else read from pkg_${LANGUAGE_CODE}.xml
          if [ -n "${{ github.event.inputs.version }}" ]; then
            VERSION="${{ github.event.inputs.version }}"
          else
            VERSION=$(xmllint --xpath "string(//version)" pkg_${LANGUAGE_CODE}.xml)
          fi

          PACKAGE_NAME="${EXTENSION_NAME}-${BRANCH_NAME}"
          echo "branch=${BRANCH_NAME}" >> $GITHUB_OUTPUT
          echo "version=${VERSION}" >> $GITHUB_OUTPUT
          echo "package_name=${PACKAGE_NAME}" >> $GITHUB_OUTPUT

      - name: Create individual zip files
        run: |
          mkdir -p build/zips
          zip -r build/zips/admin_${LANGUAGE_CODE}.zip ${LANGUAGE_CODE}/admin_${LANGUAGE_CODE}
          zip -r build/zips/api_${LANGUAGE_CODE}.zip ${LANGUAGE_CODE}/api_${LANGUAGE_CODE}
          zip -r build/zips/site_${LANGUAGE_CODE}.zip ${LANGUAGE_CODE}/site_${LANGUAGE_CODE}

      - name: Create final package zip
        run: |
          mkdir -p build/final
          cp pkg_${LANGUAGE_CODE}.xml build/zips/
          cd build/zips
          zip -r ../final/${{ steps.vars.outputs.package_name }}.zip ./*
          cd ../..

      - name: Generate SHA256 and SHA512 hashes
        run: |
          cd build/final
          sha256sum ${{ steps.vars.outputs.package_name }}.zip > ${{ steps.vars.outputs.package_name }}.zip.sha256
          sha384sum ${{ steps.vars.outputs.package_name }}.zip > ${{ steps.vars.outputs.package_name }}.zip.sha384
          sha512sum ${{ steps.vars.outputs.package_name }}.zip > ${{ steps.vars.outputs.package_name }}.zip.sha512
          cd ../..

      - name: Upload package and hashes as artifacts
        uses: actions/upload-artifact@v4
        with:
          name: ${{ steps.vars.outputs.package_name }}
          path: |
            build/final/${{ steps.vars.outputs.package_name }}.zip
            build/final/${{ steps.vars.outputs.package_name }}.zip.sha256
            build/final/${{ steps.vars.outputs.package_name }}.zip.sha384
            build/final/${{ steps.vars.outputs.package_name }}.zip.sha512

      - name: Commit to download folder (optional)
        if: github.ref == 'refs/heads/joomla5' || github.ref == 'refs/heads/joomla6'
        run: |
          git config --global user.name "github-actions"
          git config --global user.email "actions@github.com"
          git fetch
          git checkout ${{ steps.vars.outputs.branch }}
          mkdir -p downloads
          cp build/final/* downloads/
          git add -f downloads/
          git commit -m "Add new package ${{ steps.vars.outputs.package_name }}"
          git push

      - name: Install xmlstarlet
        run: sudo apt-get update && sudo apt-get install -y xmlstarlet

      - name: Update updates.xml with current metadata
        run: |
          PACKAGE_NAME="${{ steps.vars.outputs.package_name }}"
          ZIP_PATH="build/final/${PACKAGE_NAME}.zip"

          VERSION="${{ steps.vars.outputs.version }}"
          SHA256=$(cut -d' ' -f1 build/final/${PACKAGE_NAME}.zip.sha256)
          SHA384=$(cut -d' ' -f1 build/final/${PACKAGE_NAME}.zip.sha384)
          SHA512=$(cut -d' ' -f1 build/final/${PACKAGE_NAME}.zip.sha512)
          BRANCH="${{ steps.vars.outputs.branch }}"

          DOWNLOAD_URL="https://github.com/ceford/cefjdemos-pkg-${LC_LANGUAGE_CODE}/raw/${BRANCH}/downloads/${PACKAGE_NAME}.zip"
          CHANGELOG_URL="https://raw.githubusercontent.com/ceford/cefjdemos-pkg-${LC_LANGUAGE_CODE}/${BRANCH}/changelog.xml"

          xmlstarlet ed \
            -u "//update/version" -v "$VERSION" \
            -u "//update/downloads/downloadurl" -v "$DOWNLOAD_URL" \
            -u "//update/sha256" -v "$SHA256" \
            -u "//update/sha384" -v "$SHA384" \
            -u "//update/sha512" -v "$SHA512" \
            -u "//update/changelogurl" -v "$CHANGELOG_URL" \
            updates.xml > updated_updates.xml

          mv updated_updates.xml updates.xml

      - name: Commit updated updates.xml
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"

          git add updates.xml
          git commit -m "Update updates.xml with version ${{ steps.vars.outputs.version }}" || echo "No changes to commit"
          git push origin ${{ steps.vars.outputs.branch }}
```

### The updates.xml file

The pkg_gd-GB.xml file is copied into the installation zip file during the build process. It contains the url of the update server - the link to the latest `updates.xml` file. This allows the Joomla installer to check the hash value of an update before unpacking and installing.

**Important:** This file will change on every update.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<updates>
  <update>
    <name>Scottish Gaelic</name>
    <description>Scottish Gaelic Language Pack</description>
    <element>gd-GB</element>
    <type>language</type>
    <version>5.3.1.2</version>
    <client>administrator</client>
    <infourl title="Scottish Gaelic Language Pack">https://github.com/ceford/cefjdemos-pkg-gd-gb/blob/main/README.md</infourl>
    <downloads>
      <downloadurl type="full" format="zip">https://github.com/ceford/cefjdemos-pkg-gd-gb/raw/joomla5/downloads/pkg_gd-GB-joomla5.zip</downloadurl>
    </downloads>
    <sha256>f21ed31c4453e74e0be83e279ba090965714a6f3d8fe2ca97d08aa415e4efb9a</sha256>
    <sha384>a06602deb5abed5f5560f496732c639dea97a4bc900599662428784ee646b3b5689da3e36d92ba59c3ea47ce00080bbd</sha384>
    <sha512>79c8ad3714512cabe16602097f0a33844249ae6d8d4468fae2e1ac8f0ee185e647d106089a5c375f79a1c2d0cbad7bde610837665f4b7fa26b20cd5cde3058d6</sha512>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-pkg-gd-gb/joomla5/changelog.xml</changelogurl>
    <tags>
      <tag>stable</tag>
    </tags>
    <targetplatform name="joomla" version="[45].[012345]"/>
  </update>
</updates>
```

## JED Checker

Although this extension is not destined for the Joomla Extensions Directory, the JED Checker is an invaluable development tool. This is what it reports:

![JED Checker Screenshot](../../../en/images/languages/jed-check-gaelic.png)

The XML Manifest errors appear to be a JED Checker bug that has been reported. It says:

```html
#001 /pkg_gd-GB.xml
The node <file> has attribute 'client' with unknown value "api"
```

## Result

If you would like to try out Scottish Gaelic you can obtain the [package](https://github.com/ceford/cefjdemos-pkg-gd-gb/raw/joomla5/downloads/pkg_gd-GB-joomla5.zip) from the GitHub

The following screenshot shows the Home Dashboard with Scottish Gaelic as the Administrator language:

![Home dashboard in Gaelic](../../../en/images/languages/home-dashboard-gaelic.png)

You may notice that some words are in English! They are the module headings that were entered in English. The modules could be edited and the module titles changed to the default language.
