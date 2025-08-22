<!-- Filename: J4.x:Developer:_Required_Software / Display title: Language Extension Example -->

## Introduction

Official Joomla language extensions are normally installed via the System &rarr; Install &rarr; Languages route. However, there may be occasions when it is necessary to install a language extension via the Install &rarr; Extensions &rarr; Upload & Install route. This example is for Scottish Gaelic with all of the English to Gaelic translation obtained using openai.com at a cost of just under $5. It is an unofficial language extension because it really needs the translations verified by Gaelic speakers, of which there are about 60,000 in total. Creation of the extension was inspired by the coincidence of an enquiry in the Forum and a personal visit to the ruins of Carnasserie Castle where the very first printed document in Scottish Gaelic was produced in 1567.

## Local Repository File Structure

The following local repository structure contains `.ini` files already translated from the original English `.ini` files. The method of translation is covered in a separate [article](jdocmanual?article=developer/languages/translate-openai).

```sh
cefjdemos-pkg-gd-gb
    .github
        workflows
            package.yml
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

Also, an extra branch named gh-pages is created by a GitHub Workflow. It is used to store links to the latest versions and to update information.

## The pkg_gd-GB.xml File

Note that **gd-GB** is the ISO code for Scottish Gaelic. Most of the fields in the pkg_gd-GB.xml file are self-explanatory. It is possible to create separate Site and Administrator language extensions. However, the Administrator `install.xml` and `langmetadata.xml` files are required for language administration and the `localise.php` file is required for use by some plugins.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="package" method="upgrade">
	<name>Scottish Gaelic</name>
	<packagename>gd-GB</packagename>
	<version>5.3.1.3</version>
	<creationDate>2025-06-18</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@fford.me.uk</authorEmail>
	<authorUrl>https://github.com/ceford/</authorUrl>
	<copyright>(C) 2025 Clifford E Ford. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<url>https://github.com/ceford/cefjdemos-pkg-gd-gb</url>
	<packager>Clifford E Ford</packager>
	<packagerurl>https://github.com/ceford/</packagerurl>
	<description><![CDATA[Scottish Gaelic language pack with translation by openai.com]]></description>
	<blockChildUninstall>true</blockChildUninstall>
	<files>
		<file type="language" client="site" id="gd-GB">site_gd-GB.zip</file>
		<file type="language" client="administrator" id="gd-GB">admin_gd-GB.zip</file>
		<file type="language" client="api" id="gd-GB">api_gd-GB.zip</file>
	</files>
	<updateservers>
		<server type="extension" priority="2" name="Scottish Gaelic Update Site">https://ceford.github.io/cefjdemos-pkg-gd-gb/joomla5/updates.xml</server>
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
	<version>5.3.1.3</version>
	<creationDate>2025-06-18</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@fford.me.uk</authorEmail>
	<authorUrl>https://github.com/ceford/</authorUrl>
	<copyright>(C) 2025 Clifford E Ford. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<description>Scottish Gaelic administrator language</description>
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
	<version>5.3.1.3</version>
	<creationDate>2025-06-18</creationDate>
	<author>Clifford E Ford</author>
	<authorEmail>cliff@fford.me.uk</authorEmail>
	<authorUrl>https://github.com/ceford/</authorUrl>
	<copyright>(C) 2025 Clifford E Ford. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<description>Scottish Gaelic administrator language</description>
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

The files in these folders are similar to those in the admin folder except that the client attribute is set to `api` and `site` respectively and there are fewer `*.ini` files. The files are not reproduced here. See the en-GB versions for examples.

## The GitHub Workflow

Package creation involves several steps:

- Create individual zip files for each client (administrator, api and site).
- Create final package zip.
- Generate SHA hashes.
- Save package and hashes for public access.
- Update the `updates.xml` file with current metadata.

This work is accomplished on GitHub with the .github/workflows/package.yml file. It is triggered every time there is a commit to the repository. 

### The package.yml file

Development of the package.yml file took several days and considerable help from ChatGPT.

Some explanatory notes:

- `on: push: branches:` 
    - A run is triggered whenever a change to the joomla5 or joomla6 branch is pushed from the local repository to the GitHub repository.
- `permissions: contents: write`
    - write permissions are required to allow the workflow to save the package zip file.
- `env: EXTENSION_NAME: pkg_gd-GB` 
    - Items that change from one language pack to another are stored as environment variables. To change to another language set the language specific variables here. No other changes are required below this point.
    - Except that I should have set the GitHub repository url in an environment variable too.
- `jobs: publish: runs-on: ubuntu`
    - This is a standard command to run a job (publish or build or whatever you want to call it)
- `steps:`
    - These are the individual steps required to build the extension. Read through them to see what happens. Note the steps that create individual zip files for each Joomla client (`administrator`, `api` and `site`) followed by the step that combines them into a downloadable zip file. Then comes generation of `SHA` values and a GitHub release.
    - `name Debug URLs run:` This is just an example of code planted for debugging purposes. Debug statements could be anywhere in the `run:` sequence.
    - `name: Install xmlstarlet`
        - This step and the following one make an updated copy of the `updates.xml` file copied into a `dist` folder. This means that your local `updates.xml` will never contain the correct SHA values or version link. **Leave it like that!** Otherwise your local and remote repos will become out of sync and require troublesome manual syncing.
    - `name: Clone gh-pages into ./publish`
        - The remaining steps populate or update GitHub pages, which is where you keep the updates.xml file and any other information about the extension that you care to mention.
- Creation of GitHub pages is covered later.
- After pushing a change to GitHub it can take a few minutes for the script to build a revised release.

```yml
name: Build and Release Joomla Language Packages

on:
  push:
    branches:
      - joomla5  # or your main development branch
      - joomla6

permissions:
  contents: write
  pages: write
  id-token: write


env:
  EXTENSION_NAME: pkg_gd-GB
  LANGUAGE_CODE: gd-GB
  LC_LANGUAGE_CODE: gd-gb
  LANGUAGE_NAME: Scottish Gaelic
  # Joomla Versions
  VERSION5: 5.3.2
  VERSION6: 6.0.0
  # Language Pack Versions
  LP_VERSION5: 5.3.1.4
  LP_VERSION6: 6.0.0.0

jobs:
  publish:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Install xmllint
        run: sudo apt-get update && sudo apt-get install -y libxml2-utils

      - name: Set branch and version
        id: set_version
        run: |
          BRANCH_NAME="${GITHUB_REF_NAME}"
          echo "Branch is $BRANCH_NAME"
          if [ "$BRANCH_NAME" = "joomla5" ]; then
            VERSION=${VERSION5}
            LP_VERSION=${LP_VERSION5}
            PACKAGE_NAME="${EXTENSION_NAME}-joomla5"
          elif [ "$BRANCH_NAME" = "joomla6" ]; then
            VERSION=${VERSION6}
            LP_VERSION=${LP_VERSION6}
            PACKAGE_NAME="${EXTENSION_NAME}-joomla6"
          else
            echo "Unknown branch $BRANCH_NAME"
            exit 1
          fi
          echo "BRANCH_NAME=$BRANCH_NAME" >> $GITHUB_ENV
          echo "VERSION=$VERSION" >> $GITHUB_ENV
          echo "PACKAGE_NAME=$PACKAGE_NAME" >> $GITHUB_ENV
          echo "LP_VERSION=$LP_VERSION" >> $GITHUB_ENV

      # Force tag update
      - name: Force create/update tag
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git tag -f $VERSION
          git push origin -f $VERSION
        env:
          VERSION: ${{ env.VERSION }}

      # (Optional) sanity check that zip is installed
      - name: Ensure zip is available
        run: |
          if ! command -v zip >/dev/null 2>&1; then
            sudo apt-get update && sudo apt-get install -y zip
          fi

      - name: Create individual zip files
        run: |
          mkdir -p build/zips
          zip -r build/zips/admin_${LANGUAGE_CODE}.zip ${LANGUAGE_CODE}/admin_${LANGUAGE_CODE}
          zip -r build/zips/api_${LANGUAGE_CODE}.zip ${LANGUAGE_CODE}/api_${LANGUAGE_CODE}
          zip -r build/zips/site_${LANGUAGE_CODE}.zip ${LANGUAGE_CODE}/site_${LANGUAGE_CODE}

      - name: Create final package zip
        run: |
          set -euxo pipefail
          mkdir -p build/final

          # Show what we’re about to package
          ls -l build/zips || true

          # Ensure the XML is included alongside the three zips
          cp "pkg_${LANGUAGE_CODE}.xml" build/zips/

          # Create the final zip FROM ABSOLUTE PATHS (no cd needed)
          # -j strips the directory paths so contents are at the root of the archive
          zip -j "build/final/${PACKAGE_NAME}.zip" \
            build/zips/*.zip \
            "build/zips/pkg_${LANGUAGE_CODE}.xml"

          echo "Final contents:"
          ls -l build/final

      - name: Generate SHA256 and SHA512 hashes
        run: |
          cd build/final
          sha256sum ${PACKAGE_NAME}.zip > ${PACKAGE_NAME}.zip.sha256
          sha384sum ${PACKAGE_NAME}.zip > ${PACKAGE_NAME}.zip.sha384
          sha512sum ${PACKAGE_NAME}.zip > ${PACKAGE_NAME}.zip.sha512
          cd ../..

      # Create or update the GitHub release
      - name: Create or update GitHub release
        uses: softprops/action-gh-release@v1
        with:
          tag_name: ${{ env.VERSION }}
          name: ${{ env.PACKAGE_NAME }}
          draft: false
          prerelease: ${{ contains(env.BRANCH_NAME, 'joomla6') }}
          files: |
            build/final/${{ env.PACKAGE_NAME }}.zip
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Set download URLs for updates.xml
        id: set_urls
        run: |
          BASE_URL="https://github.com/${GITHUB_REPOSITORY}/releases/download/${VERSION}"
          echo "ZIP_URL=${BASE_URL}/${PACKAGE_NAME}.zip" >> $GITHUB_ENV
          echo "SHA256_URL=${BASE_URL}/${PACKAGE_NAME}.zip.sha256" >> $GITHUB_ENV
          echo "SHA384_URL=${BASE_URL}/${PACKAGE_NAME}.zip.sha384" >> $GITHUB_ENV
          echo "SHA512_URL=${BASE_URL}/${PACKAGE_NAME}.zip.sha512" >> $GITHUB_ENV

      - name: Debug URLs
        run: |
          echo "ZIP URL: $ZIP_URL"
          echo "SHA256 URL: $SHA256_URL"
          echo "SHA384 URL: $SHA384_URL"
          echo "SHA512 URL: $SHA512_URL"
          echo "Listing build/zips/"
          ls -l build/zips/
          echo "Listing build/final/"
          ls -l build/final/

      - name: Install xmlstarlet
        run: sudo apt-get update && sudo apt-get install -y xmlstarlet

      - name: Update updates.xml with current metadata
        run: |
          ZIP_PATH="build/final/${PACKAGE_NAME}.zip"

          SHA256=$(cut -d' ' -f1 build/final/${PACKAGE_NAME}.zip.sha256)
          SHA384=$(cut -d' ' -f1 build/final/${PACKAGE_NAME}.zip.sha384)
          SHA512=$(cut -d' ' -f1 build/final/${PACKAGE_NAME}.zip.sha512)

          DOWNLOAD_URL=$ZIP_URL

          xmlstarlet ed \
            -u "//update/version" -v "$LP_VERSION" \
            -u "//update/downloads/downloadurl" -v "$DOWNLOAD_URL" \
            -u "//update/sha256" -v "$SHA256" \
            -u "//update/sha384" -v "$SHA384" \
            -u "//update/sha512" -v "$SHA512" \
            updates.xml > updated_updates.xml

          mkdir -p dist
          mv updated_updates.xml dist/updates.xml

        # after you build your artifacts (zips, hashes, updates.xml) into ./dist
        # e.g. dist/updates.xml, dist/*.zip, dist/*.sha256

      - name: Clone gh-pages into ./publish
        env:
            GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
            set -e
            REPO="https://x-access-token:${GH_TOKEN}@github.com/${{ github.repository }}.git"

            if git ls-remote --heads "$REPO" gh-pages | grep -q gh-pages; then
              echo "Cloning existing gh-pages branch"
              git clone --depth 1 --branch gh-pages "$REPO" publish
            else
              echo "Creating new gh-pages branch"
              git init publish
              cd publish
              git checkout -b gh-pages
              git remote add origin "$REPO"
              touch .nojekyll
              git -c user.name="github-actions[bot]" -c user.email="github-actions[bot]@users.noreply.github.com" \
                commit --allow-empty -m "Initialize gh-pages"
              git push -u origin gh-pages
              cd ..
            fi

      - name: Update files in gh-pages
        run: |
            set -e
            BRANCH=${GITHUB_REF_NAME}     # "joomla5" or "joomla6"
            mkdir -p publish/$BRANCH

            # Copy your branch artifacts
            cp dist/updates.xml publish/$BRANCH/
            # If you also generate zips + hashes, include them:
            # cp dist/*.zip publish/$BRANCH/ || true
            # cp dist/*.sha256 publish/$BRANCH/ || true

            # (Re)write index.html
            cat <<EOF > dist/index.html
            <!doctype html>
            <html>
            <head>
                <meta charset="utf-8">
                <title>${LANGUAGE_NAME}</title>
                <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/water.css@2/out/water.css">
            </head>
            <body>
            <h1>${LANGUAGE_NAME} Language Package</h1>
            <h2>Download for Installation</h2>
            <ul>
              <li><a href="https://github.com/ceford/cefjdemos-pkg-${LC_LANGUAGE_CODE}/releases/download/${VERSION5}/pkg_${LANGUAGE_CODE}-joomla5.zip">Joomla 5 Installation ZIP</a></li>
              <li><a href="https://github.com/ceford/cefjdemos-pkg-${LC_LANGUAGE_CODE}/releases/download/${VERSION6}/pkg_${LANGUAGE_CODE}-joomla6.zip">Joomla 6 Installation ZIP</a></li>
            </ul>
            <h2>Update Files</h2>
            <p>These files are used by installations to obtain update information. They are not
            meant for human use.</p>
            <ul>
              <li><a href="./joomla5/updates.xml">Joomla 5 updates.xml</a></li>
              <li><a href="./joomla6/updates.xml">Joomla 6 updates.xml</a></li>
            </ul>
            </body>
            </html>
            EOF

            cp dist/index.html publish/

      - name: Commit and push ONLY gh-pages content
        run: |
            set -e
            git -C publish config user.name  "github-actions[bot]"
            git -C publish config user.email "github-actions[bot]@users.noreply.github.com"
            git -C publish add -A
            git -C publish commit -m "Deploy ${GITHUB_REF_NAME} at ${GITHUB_SHA}" || echo "No changes to commit"
            git -C publish push origin gh-pages
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
    <version>5.3.1.3</version>
    <client>administrator</client>
    <infourl title="Scottish Gaelic Language Pack">https://github.com/ceford/cefjdemos-pkg-gd-gb/blob/joomla5/README.md</infourl>
    <downloads>
      <downloadurl type="full" format="zip">https://github.com/ceford/cefjdemos-pkg-gd-gb/releases/download/5.3.2/pkg_gd-GB-joomla5.zip</downloadurl>
    </downloads>
    <sha256>191814bd8ee4115a201d768c41ddcc9d182b116c2fa7a20207c709449bb44ffd</sha256>
    <sha384>213502c527b2a7b16bec85ca412e7bcadcfeebb343a98515cff5acb306e80bd0141a418b4dd992bd939b042b4b173774</sha384>
    <sha512>6302aea1ec3ba5ce412e63f2bef6aee40fa14a76d69144a925507e611b88b5881a6100d51f1d09ac1f82775ede66dcff05f256c41f9b8f9a4b6fe37682aebb09</sha512>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-pkg-gd-gb/refs/heads/joomla5/changelog.xml</changelogurl>
    <tags>
      <tag>stable</tag>
    </tags>
    <targetplatform name="joomla" version="[45].[012345]"/>
  </update>
</updates>
```

### The changelog.xml file

A format of changelog files is described in a [separate article](jdocmanual?article=docus/building-extensions/install-update-installation-change-log). The convention I have used for custom language packs use the Joomla version number for which the translation was obtained followed by an extra digit to indicate the language package revision number. 

```
<changelogs>
    <changelog>
        <element>gd-GB</element>
        <type>language</type>
        <version>5.3.1.3</version>
        <add>
            <item>First release</item>
        </add>
    </changelog>
</changelogs>
```

## GitHub Pages

GitHub allows you to create a website for your repositories. There are two main types of GitHub Pages sites you can create:

1. User / Organization Site
    - URL format: `https://username.github.io/`
    - Repository required: A repo named username.github.io (exact name).
    - How it gets created:
        - You must create this repo explicitly (via GitHub UI or by pushing to it).
        - Sometimes a workflow in another repo can create and populate it automatically by pushing built files into a new username.github.io repo — this may have happened in your case.
    - Typical setup:
        - Create repo username.github.io.
        - Add an index.html (or push built files into it).
        - GitHub Pages automatically serves it at `https://username.github.io`.
2. Project Site
    - URL format: `https://username.github.io/repository-name/`
    - Repository used: Any repo (e.g. cefjdemos-pkg-gd-gb).
    - How it gets published:
        - Enable Pages in repo settings.
        - Choose `gh-pages` branch (or /docs folder in main) as the source.
        - Add an `index.html` there.
    - Typical setup:
        - In repo `your-project`, create a `gh-pages` branch (or use /docs folder).
        - Place an index.html inside.
        - Enable Pages in repo Settings → Pages.
        - Visit `https://username.github.io/repository-name/`.

### For the language pack repository:

- Got to to the repository page.
- Select `Settings` at the top of the page.
- Select `Pages` at from the left column of the `Settings` page.
- Select `Deploy from a branch`from the Build and deployment `Source` list.
- Select `gh-pages` from the list of branches.
- Select the `Save` button.

![github project page creation](../../../en/images/languages/github-project-page.png)

This will populate the project page using data from the gh-pages branch in the project repository. After saving the link will be visible in the page.

## JED Checker

Although this extension is not destined for the Joomla Extensions Directory, the JED Checker is an invaluable development tool. This is what it reports:

![JED Checker Screenshot](../../../en/images/languages/jed-check-gaelic.png)

The XML Manifest errors appear to be a JED Checker bug that has been reported. It says:

```html
#001 /pkg_gd-GB.xml
The node <file> has attribute 'client' with unknown value "api"
```

## Result

If you would like to try out Scottish Gaelic you can obtain the link to the current [package](https://ceford.github.io/cefjdemos-pkg-gd-gb/) version from the GitHub.

The following screenshot shows the Home Dashboard with Scottish Gaelic as the Administrator language:

![Home dashboard in Gaelic](../../../en/images/languages/home-dashboard-gaelic.png)

You may notice that some words are in English! They are the module headings that were entered in English. The modules could be edited and the module titles changed to the default language.
