<!-- Filename: https://docs.joomla.org/Package / Display title: Packages -->

## Introduction to Packages

Sometimes a module (or plugin or other extension) uses a component's models or helpers. On these occasions it is best to be able to install or uninstall a dependent module when the component itself is installed or uninstalled. In Joomla, this functionality is achieved with a package, although the use of packages is not foolproof.

A package is an extension that is used to install multiple extensions at one time. Combining them in a package allows the user to install and uninstall two or more extensions in a single operation.

## Package Creation

A package is created by zipping all of the individual extension `.zip` files together with an `.xml` manifest file. For example, for a package composed of:

* **component** helloworld
* **module** helloworld
* **library** helloworld
* **system plugin** helloworld
* **template** helloworld

The package would have the following tree in the zipfile:

```sh
  -- pkg_helloworld.xml
  -- packages <dir>
      |-- com_helloworld.zip
      |-- mod_helloworld.zip
      |-- lib_helloworld.zip
      |-- plg_sys_helloworld.zip
      |-- tpl_helloworld.zip
  -- pkg_script.php
```

The `pkg_helloworld.xml` could have the following contents:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<extension type="package" method="upgrade">
<name>Hello World Package</name>
<author>Hello World Package Team</author>
<creationDate>May 2022</creationDate>
<packagename>helloworld</packagename>
<version>1.0.0</version>
<url>http://www.yoururl.com/</url>
<packager>Hello World Package Team</packager>
<packagerurl>http://www.yoururl.com/</packagerurl>
<description>Example package to combine multiple extensions</description>
<update>http://www.updateurl.com/update</update>
<scriptfile>pkg_script.php</scriptfile>
<blockChildUninstall>true</blockChildUninstall>
<files folder="packages">
    <file type="component" id="com_helloworld">com_helloworld.zip</file>
    <file type="module" id="helloworld" client="site">mod_helloworld.zip</file>
    <file type="library" id="helloworld">lib_helloworld.zip</file>
    <file type="plugin" id="helloworld" group="system">plg_sys_helloworld.zip</file>
    <file type="template" id="helloworld" client="site">tpl_helloworld.zip</file>
</files>
</extension>
```

When zipped and installed, the package will be visible in the extension list so that a user might uninstall all extensions contained in the package.

Remember to use the package uninstaller instead of individual subpackage uninstallers to avoid orphan extension entries in the extension manager. This is the part that is not foolproof!

### File id

The id element in the `<file ..>` tag is **not** arbitrary! The `id=` should be set to the value of the `element` column in the `#__extensions` table. If they are not set correctly, upon uninstallation of the package, the child `file` will **not** be found and uninstalled.

### Manifest Filename and Packagename

The naming of the manifest file and the ability to uninstall the package file itself are closely related. The manifest file must have a **pkg_** prefix. The remainder of the manifest name (without the **.xml** extension) is to be used as `<packagename>`. Or, the other way around, a package you want to identify as **blurpblurp_J3** gets that as its `<packagename>` and should be in a manifest file named **pkg_blurpblurp_J3.xml**. Failure to do so will make it impossible to uninstall the package itself.

An optional `<pkg_script.php>` which contains a class `pkg_<packagename>InstallerScript` with public function **postflight** can be used.

### Extension Tag

The `<extension>` tag should include `method="upgrade"` for a package update to succeed on subsequent installations.

### Extension Uninstall Dependency

A package extension can declare that its child elements cannot be uninstalled independently with a `<blockChildUninstall>true</blockChildUninstall>` element in the package manifest. If your package needs all its extensions to operate effectively, set this to true or 1.
