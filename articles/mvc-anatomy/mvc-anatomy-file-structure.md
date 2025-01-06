<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: MVC Anatomy: File Structure -->

## Developer Setup

There are several ways to organise files for development purposes. One method is to use a clone of a GitHub repository as in the following schematic structure:

```
cefjdemos-com-countrybase
|-- .vscode (folder for build settings)
|-- com_countrybase (folder compressed to create an installable zip file)
    |-- admin (the administrator files)
        |-- forms
        |-- help
            |-- countrybase.html (this is a Help screen used in the backend module edit form)
        |-- language
            |-- en-GB (language folder, kept with the extension code)
                |-- mod_downmsg.ini
                |-- mod_downmsg.sys.ini
        |-- services (folder for dependency injection)
            |-- provider.php (the DI code)
        |-- sql
            |-- updates
                |-- mysql
                    |-- index.html (an empty file required to avoid an installation error if there are no sql updates)
        |-- src (folder for namespaced classes)
            |-- more folders: Controller, Extension, Helper, Model, Table and View
        |-- tmpl (folder for layouts)
            |-- more folders: countries, country, currencies and currency
        |-- access.xml (standard list of access permissions)
        |-- config.xml (options parameters)
    |-- media
        |-- css (placeholder for CSS files - contains an empty index.html file)
        |-- js (placeholder for JavaScript files - contains an empty index.html file)
    |-- site (the site files, abbreviated here)
        |-- forms
        |-- language
        |-- src
        |-- tmpl
    |-- countrybase.xml (the manifest file)
    |-- script.php (a script to run on install, update or uninstall)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- changelog.xml (record of changes)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
|-- updates.xml (update server specification)
```

This is the structure in the VSCodium IDE:

![Vscodium file structure view](../../../en/images/mvc-anatomy/com-countrybase-vscodium.png)

On installation the parts of the `com_countrybase` component are distributed to different locations in the Joomla file structure:
- Administrator files go into `root/administrator/components/com_countrybase`.
- Site files go into `root/components/com_countrybase`.
- Media files, javascript and css files (if any), go into `root/media/com_countrybase`.
- Language files go into the administrator and site component structures. A previous convention placed them in the core language folders.

Just where the items go is controlled by the component manifest file, in this case countrybase.xml.

Note that the zip file contains everything inside the com_countrybase folder. You can make a zip just by compressing that folder. Outside that folder are build.xml, which is a file used to build the component whenever a change is made, and README.md, which is a standard Markdown file in Github format that describes the component.

## Notes

- There are more than 40 files in this simple component!
- On installation the countrybase.xml file is copied to administrator/components/com_countrybase where it is needed for update or uninstall purposes.
