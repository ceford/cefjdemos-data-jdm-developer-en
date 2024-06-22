<!-- Filename: Packgea / Display title: Package -->

## Note on the Component Dependent Modules

Sometimes modules (or plugins and so on) use a component's models and helpers and such. On these occasions it would be nice to be able to package your module in a way that it gets uninstalled when the component itself gets uninstalled. This is called dependency management that has been desired in Joomla but hasn't been done yet.
It is suggested that you follow the standards and use a package as described below.

## What is a Package?

A package is a extension that is used to install multiple extensions at one time. Use a package if you have, for example, a component and a module that are dependent upon each other. Combining them in a package will allow the user to install and uninstall both extensions in a single operation. 

## How Do I Create a Package?

A package extension is created by zipping all ''.zip'' files of the extensions together with a ''.xml'' manifest file. For example, if you have a package composed of:

* **component** helloworld
* **module** helloworld
* **library** helloworld 
* **system plugin** helloworld 
* **template** helloworld

The package should have the following tree in your zipfile:

```bash
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

The ''pkg_helloworld.xml'' could have the following contents:

```xml
 <?xml version="1.0" encoding="UTF-8" ?>
 <extension type="package" version="1.6" method="upgrade">
 <name>Hello World Package</name>
 <author>Hello World Package Team</author>
 <creationDate>May 2012</creationDate>
 <packagename>helloworld</packagename>
 <version>1.0.0</version>
 <url>http://www.yoururl.com/</url>
 <packager>Hello World Package Team</packager>
 <packagerurl>http://www.yoururl.com/</packagerurl>
 <description>Example package to combine multiple extensions</description>
 <update>http://www.updateurl.com/update</update>
 <scriptfile>pkg_script.php</scriptfile>
 <files folder="packages">
   <file type="component" id="com_helloworld" >com_helloworld.zip</file>
   <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
   <file type="library" id="helloworld">lib_helloworld.zip</file>
   <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
   <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
 </files>
 </extension>
```

When you zip this and install it, you will see that all extensions will be installed. The package will be visible in the extension list so that a user might uninstall all extensions contained in the package. 

Remember to use the package uninstaller instead of individual subpackage uninstallers to avoid orphan extension entries in the extension manager.

### File id

The id element in the `<file ..>` tag is **not** arbitrary! The `id=` should be set to the value of the `element` column in the `#__extensions` table. If they are not set correctly, upon uninstallation of the package, the child `file` will **not** be found and uninstalled.

### Manifest Filename and Packagename

The naming of the manifest file and the ability to uninstall the package file itself are closely related.
The manifest file must have a **pkg_** prefix. The remainder of the manifest name (without the **.xml** extension) is to be used as `<packagename>`. Or, the other way around, a package you want to identify as **blurpblurp_J3** gets that as its `<packagename>` and should be in a manifest file named **pkg_blurpblurp_J3.xml**. Failure to do so will make it impossible to uninstall the package itself.

An optional `<pkg_script.php>` which contains a class `pkg_<packagename>InstallerScript` with public function **postflight** can be used.

### Extension Tag

As of Joomla! 3.4, you should include `method="upgrade"` in the `<extension>` tag if you want your package to succeed in updating itself on subsequent installations.

The version attribute of the `<extension>` tag should be set to the minimum supported version of Joomla for this package. For example, if your package only supports Joomla! 3.2 and above, set this to 3.2.

Note: This `version` attribute is purely optional. It has no influence on Joomla's installation routine. As of Joomla 4, the `version` attribute has therefore been removed from all core extensions.

### Extension Uninstall Dependency

As of Jooma! 3.7.0, a package extension can declare that its child elements cannot be uninstalled independently with a `<blockChildUninstall>true</blockChildUninstall>` element in the package manifest. If your package needs all its extensions to operate effectively, set this to true or 1.

Source: https://docs.joomla.org/Package
