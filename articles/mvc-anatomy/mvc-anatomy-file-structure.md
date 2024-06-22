<!-- Filename: J4.x:MVC_Anatomy:_File_Structure / Display title: MVC Anatomy: File Structure -->

## Component Zip File Structure

On installation the parts of the come_countrybase component are
distributed to different locations in the Joomla file structure.
Administrator files go into
root/administrator/components/com_countrybase. Site files go into
root/components/com_countrybase. Media files, javascript and css files
(if any), go into root/media/com_countrybase. In some components
language files may go into root/language/en-GB/com_countrybase.ini but
the latest recommendation is to keep the language files in a subfolder
of the component.

Just where the items go is controlled by the component manifest file, in
this case countrybase.xml.

The component zip file structure is illustrated below as it appears in
the Eclipse IDE:

<img
src="https://docs.joomla.org/images/thumb/0/01/J4x-mvc-anatomy-component-file-structure-en.png/800px-J4x-mvc-anatomy-component-file-structure-en.png"
decoding="async"
srcset="https://docs.joomla.org/images/0/01/J4x-mvc-anatomy-component-file-structure-en.png 1.5x"
data-file-width="1200" data-file-height="661" width="800" height="441"
alt="com_countrybase File Structure" />

Note that the zip file contains everything inside the com_countrybase
folder. You can make a zip just by compressing that folder. Outside that
folder are build.xml, which is a file used to build the component
whenever a change is made, and README.md, which is a standard Markdown
file in Github format that describes the component.

## Notes

- There are more than 40 files in this simple component!
- Files destined for the administrator, media and site locations are
  shown expanded.
- On installation the countrybase.xml file is copied to
  administrator/components/com_countrybase where it is needed for update
  or uninstall purposes.

## Related Tutorials

### MVC Anatomy

- [Getting
  Started](https://docs.joomla.org/J4.x:MVC_Anatomy:_Getting_Started "Special:MyLanguage/J4.x:MVC Anatomy: Getting Started")
- [File
  Structure](https://docs.joomla.org/J4.x:MVC_Anatomy:_File_Structure "Special:MyLanguage/J4.x:MVC Anatomy: File Structure")
- [Manifest
  File](https://docs.joomla.org/J4.x:MVC_Anatomy:_Manifest_File "Special:MyLanguage/J4.x:MVC Anatomy: Manifest File")
- [Site
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Site_Files "Special:MyLanguage/J4.x:MVC Anatomy: Site Files")
- [Administrator Startup
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Administrator_Startup_Files "Special:MyLanguage/J4.x:MVC Anatomy: Administrator Startup Files")
- [Administrator Edit
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Administrator_Edit_Files "Special:MyLanguage/J4.x:MVC Anatomy: Administrator Edit Files")
