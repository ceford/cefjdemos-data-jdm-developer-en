<!-- Filename: J4.x:Developing_an_MVC_Component/Setting_up_the_Database / Display title: Developing an MVC Component/Setting up the Database -->

Joomla!  4.x \>Tutorial Setting Up the Database

**Articles in this Series**

1.  [Introduction](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Introduction "Special:MyLanguage/J4.x:Developing an MVC Component/Introduction")
2.  [Developing a Basic
    Component](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Developing_a_Basic_Component "Special:MyLanguage/J4.x:Developing an MVC Component/Developing a Basic Component")
3.  [Adding a View to the Site
    Part](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_View_to_the_Site_Part "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a View to the Site Part")
4.  [Adding a Menu Type to the Site
    Part](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Menu_Type_to_the_Site_Part "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a Menu Type to the Site Part")
5.  [Language
    Management](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Language_Management "Special:MyLanguage/J4.x:Developing an MVC Component/Language Management")
6.  [Adding a Model to the Site
    Part](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Model_to_the_Site_Part "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a Model to the Site Part")
7.  [Adding a Request Variable in the Menu
    Link](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Request_Variable_in_the_Menu_Link "Special:MyLanguage/J4.x:Developing an MVC Component/Adding a Request Variable in the Menu Link")
8.  [Setting Up the
    Database](https://docs.joomla.org/J4.x:Developing_an_MVC_Componenthttps://docs.joomla.org/J4.x:Developing%20an%20MVC%20Component/Setting%20up%20the%20Database)

This article is part of the tutorial ["Developing an MVC Component for
Joomla
4.x"](https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Introduction "J4.x:Developing an MVC Component/Introduction").
It is intended to be a follow-along programming tutorial, so if you have
not read the previous parts of the tutorial you are encouraged to do so.

Now we have finally arrived at the stage of development where we are
going to create the database tables that our component will use to store
and retrieve data. Depending on its purpose, your extension may not
require database tables to operate, but given the sophistication level
of component extensions this is unlikely.

Installation, uninstallation and version upgrades within a Joomla!
component are handled using SQL scripts placed in the `admin/sql` folder
of the component. An SQL script matches a specific database engine, for
example `install.mysql.utf8.sql` will only be run if Joomla! is running
on a MySQL database. At the time of writing, Joomla! 4 supports MySQL
and PostgreSQL, and we recommend writing SQL scripts for both database
types for maximum compatibility. However, for the purposes of this
tutorial we will be focusing on MySQL-based scripts.

Let's go ahead and write the SQL scripts that will be needed to create a
database table to hold customised greeting messages for our component.
In later sections of the tutorial, we'll build a control panel to manage
these messages.

## Required Changes

We'll need to make files to install and uninstall the component, and one
upgrade file. As our component is already installed, an upgrade file is
needed to make the necessary changes.

|     |                                                                                         |                                                                                                                                |
|-----|-----------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| 1   | Create: [admin/sql/install.mysql.utf8.sql](#admin.2Fsql.2Finstall.mysql.utf8.sql)       | An SQL file that is run when the component is installed on a MySQL database                                                    |
| 2   | Create: [admin/sql/uninstall.mysql.utf8.sql](#admin.2Fsql.2Funinstall.mysql.utf8.sql)   | An SQL file that is run when the component is uninstalled on a MySQL database                                                  |
| 3   | Create: [admin/sql/updates/mysql/0.0.7.sql](#admin.2Fsql.2Fupdates.2Fmysql.2F0.0.7.sql) | An SQL file that is run when the component is updated to/beyond version 0.0.7 on a MySQL database                              |
| 4   | Update: [helloworld.xml](#helloworld.xml)                                               | We'll need to add the new `sql` directory to our file copy list, and add instructions for Joomla! on where to find our scripts |

## File Details

**admin/sql/install.mysql.utf8.sql**

This file will need to be updated whenever you make changes to your
component's database schema. It is run on installation. Note that when
you are creating tables, you prefix the name with `#__`. Joomla! will
automatically replace this prefix with the system-configured database
table prefix. You must ensure that your database names use this prefix
whenever you are creating or updating them. Additionally, we **highly
recommend** that you prefix all your table names, key names, etc with
the name of your component (as seen here) to prevent any chance of
collision between components on a Joomla! site.

Note also that we are using `IF EXISTS` checks on our `DROP` statements,
and placing `DROP` statements before `CREATE` statements. While it may
seem that the status of the database is predictable - there should be no
existing table with that name before we create it - it is best to never
assume that the previous install/uninstall operation completed cleanly
and successfully. In the rare instance of such an error, our code will
continue to function.

If this file were for a PostgreSQL database, it would be called
`install.postgresql.utf8.sql`.

```sql
    DROP TABLE IF EXISTS `#__helloworld_greetings`;

    CREATE TABLE `#__helloworld_greetings` ( 
        `id` SERIAL NOT NULL, 
        `greeting` VARCHAR(200) NOT NULL, 
        `published` BOOLEAN NOT NULL DEFAULT FALSE, 
        PRIMARY KEY (`id`)
    ) ENGINE = InnoDB; 

    INSERT INTO `#__helloworld_greetings` (`greeting`) VALUES
        ('Hello World!'),
        ('Good bye World!');
```
  
**admin/sql/uninstall.mysql.utf8.sql**

This file must take every action required to remove this component's
effect on the Joomla! database. In our case, it is currently very
simple - if the greeting table exists, remove it.

If this file were for a PostgreSQL database, it would be called
`uninstall.postgresql.utf8.sql`.

```sql
    DROP TABLE IF EXISTS `#__helloworld_greetings`;
```
  
**admin/sql/updates/mysql/0.0.7.sql**

When you create a new version of your component that has matching
database changes, those changes will need to go into database update
files. The filename is simply the version number that requires the
changes, and the files are grouped into folders according to the
database engine they are intended to run on. When Joomla! upgrades a
component, it will look for any upgrade files between the previously
installed version number and the new version number, and run them in
sequence.

In our case, as we did not have any database tables before our new 0.0.7
version, this file is identical to the install file.

If this file were for a PostgreSQL database, it would be named the same,
but would be placed in `admin/sql/postgresql`.

```sql
    DROP TABLE IF EXISTS `#__helloworld_greetings`;

    CREATE TABLE `#__helloworld_greetings` ( 
        `id` SERIAL NOT NULL, 
        `greeting` VARCHAR(200) NOT NULL, 
        `published` BOOLEAN NOT NULL DEFAULT FALSE, 
        PRIMARY KEY (`id`)
    ) ENGINE = InnoDB; 

    INSERT INTO `#__helloworld_greetings` (`greeting`) VALUES
        ('Hello World!'),
        ('Good bye World!');
```
  
**helloworld.xml**

In our updated manifest file, we need to tell Joomla! to copy our new
`admin/sql` folder when installing/upgrading the component. We also need
to tell it that our component now has database files, which ones to run
on which action, and where to find them.

The three new elements, , and are fairly self-explanatory. While the
structures are slightly different, each essentially consists of an
action (the element), a database type (mysql/postgresql) and a file
path. When you add PostgreSQL support to your component, you'll add an
extra line in each element to direct Joomla! to the correct files.

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="component" version="4.0" method="upgrade">

    <name>Hello World</name>
    <!-- The following elements are optional and free of formatting constraints -->
    <creationDate>December 2020</creationDate>
    <!-- Dummy author, feel free to replace anywhere you see it-->
    <author>John Smith</author>
    <authorUrl>https://smith.ca</authorUrl>
    <copyright>John Smith</copyright>
    <license>GPL v3</license>
    <!--  The version string is recorded in the components table -->
    <version>0.0.7</version>
    <!-- The description is optional and defaults to the name -->
    <description>
        A hello world component!
    </description>

    <!-- This is the PHP namespace under which the extension's
    code is organised. It should follow this format:
    
    Vendor\Component\ComponentName

    "Vendor" can be your company or your own name
    
    The "ComponentName" section MUST match the name used 
    everywhere else for your component. Whatever the name of 
    this XML file is, the namespace must match (ignoring CamelCase). 
    -->
    <namespace path="src/">JohnSmith\Component\HelloWorld</namespace>

    <files folder="site/">
        <folder>language</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>

    <languages>
        <language tag="en-GB">site/language/en-GB/en-GB.com_helloworld.ini</language>
    </languages>

    <administration>
        <!-- The link that will appear in the Admin panel's "Components" menu -->
        <menu link="index.php?option=com_helloworld">Hello World</menu>
        <!-- List of files and folders to copy, and where to copy them -->
        <files folder="admin/">
            <folder>language</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>

        <languages>
            <language tag="en-GB">admin/language/en-GB/en-GB.com_helloworld.ini</language>
            <language tag="en-GB">admin/language/en-GB/en-GB.com_helloworld.sys.ini</language>
        </languages>
    </administration>

    <install>
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.utf8.sql</file>
        </sql>
    </install>
    <uninstall>
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.utf8.sql</file>
        </sql>
    </uninstall>
    <update>
        <schemas>
            <schemapath type="mysql">sql/updates/mysql</schemapath>
        </schemas>
    </update>

</extension>
```

## Testing the Database Installation

As always, zip up your new component version and upload it into your
Joomla! install's admin panel. It should give you a message that
installation has been successful. Nothing is actually using the table
just now, so use your preferred database client to connect into your
Joomla! database and check that the new table exists.

Next, we'll start creating a back-end admin panel to create and manage
our greeting records.

<a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Adding_a_Request_Variable_in_the_Menu_Link"
id="content-button" class="button expand success">Prev: Adding a Request
Variable in the Menu Link</a> <a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Our_First_Backend_Page"
id="content-button" class="button expand">Next: Our First Back-end
Page</a>
