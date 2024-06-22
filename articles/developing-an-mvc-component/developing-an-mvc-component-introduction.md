<!-- Filename: J4.x:Developing_an_MVC_Component/Introduction / Display title: Developing an MVC Component/Introduction -->

Joomla!  4.x \>Tutorial Developing an MVC Component for Joomla 4.x

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

  
This tutorial will teach you how to develop a component for Joomla 4.x.
A component is a type of Joomla extension that provides its own pages to
the user, rather than augmenting existing content. A component is the
most powerful type of Joomla extension, able to add a large amount of
new functionality to a site.

Joomla components are split into two main parts: an administrator part
and a site part. The administrator part provides an interface to manage
the component, and the site part provides the pages requested by users
visiting the website. This tutorial will teach you how to construct both
parts.

For a more detailed overview of Components and the Model-View-Controller
design pattern they use, please see the [Component Basics
Guide](https://docs.joomla.org/Absolute_Basics_of_How_a_Component_Functions "Special:MyLanguage/Absolute Basics of How a Component Functions").
However, please be aware that the specific file structure, naming
standards and code detailed in the guide relate to Joomla 3.x, and
should be ignored for the purposes of this tutorial.

## Requirements

You will need to have Joomla 4.x for this tutorial. Developing for
Joomla 4.x requires:

- A minimum PHP version of 7.2.5 to be installed
- Composer, to manage PHP dependencies
- NodeJS version 10 or above, to manage front-end dependencies
- A MySQL database of version 5.6 or above **or**
- A PostgreSQL database of version 11 or above
- The Git version control system

If you have not set up a local development environment, you will need to
do so before attempting this tutorial. To set up your local development
environment, please consult the [Local Environment
Guide](https://docs.joomla.org/J4.x:Setting_Up_Your_Local_Environment "Special:MyLanguage/J4.x:Setting Up Your Local Environment").

## Migrating from Joomla 3.x

For the majority of cases, migrating from Joomla 3.x to 4.x is a
straightforward process. Most of the changes needed are
naming/namespacing changes and moving files around. This author intends
to create a migration guide once this tutorial is complete, which will
be linked here.

## Contributors

- [Ribafs](https://docs.joomla.org/User:Ribafs "User:Ribafs") (Authored
  Joomla 3.x version)
- [Kevin
  Kabatra](https://docs.joomla.org/User:Kevinkabatra "User:Kevinkabatra")
  (Authored Joomla 3.x version)
- [Greg J
  Preece](https://docs.joomla.org/User:GregJPreece "User:GregJPreece")
  (Rewriting 3.x guide for 4.x)
- [Peter Baxter](https://docs.joomla.org/User:PBaxter "User:PBaxter") ()
- [Abdul
  waheed](https://docs.joomla.org/User:Sayfrndship "User:Sayfrndship")
  (Joomla 4.x component development)

<a href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component"
id="content-button" class="button expand success">Prev: Overview</a> <a
href="https://docs.joomla.org/J4.x:Developing_an_MVC_Component/Developing_a_Basic_Component"
id="content-button" class="button expand">Next: Developing a Basic
Component</a>
