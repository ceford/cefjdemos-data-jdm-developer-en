<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: J4 Plugin example - Table of Contents -->

## The J4xdemostoc Example Plugin Tutorial

## Background

MediaWiki automatically generates a nice *Table of Contents* (ToC).
There are plugins for Joomla 3.x that do the same Job but nothing yet
for Joomla 4. So for demonstration purposes I decided to create a ToC
Plugin and document the process as a tutorial. The plugin code and
working zip file are available on github:

- <a
  href="https://github.com/ceford/j4xdemos/tree/master/J4xdemos-plg-toc"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">https://github.com/ceford/j4xdemos/tree/master/J4xdemos-plg-toc</a>

There is also a helpful Joomla 4 tutorial here:

- <a href="https://docs.joomla.org/J4.x:Creating_a_Plugin_for_Joomla"
  class="external text" target="_blank"
  rel="noreferrer noopener">https://docs.joomla.org/J4.x:Creating_a_Plugin_for_Joomla</a>

## The zip file structure

Zip file structures vary a little because the destination of individual
files is decided in the manifest file. For example, if the two ini
language files shown here are at one higher level in the zip folder
structure and the en-GB folder does not exist here, then the destination
en-GB folder must be set in the manifest file. The ToC installation zip
file is structured like this:

```
    plg_j4xdemost.zip
         plg_j4xdemos_toc
              language
                   en-GB
                        en-GB.plg_content_j4xdemostc.ini
                        en-GB.plg_content_j4xdemostc.sys.ini
              j4xdemostoc.php
              j4xdemostoc.xml
```

## The manifest file

The manifest file contains extension data and it tells the install code
what goes where. As type ***plugin*** in the ***content*** group the
plugin files will be installed in site_root/plugins/content/j4xdemostoc.
The languages destination folder is site_root/administrator/language.
The ***config*** section is used to set parameters in the plugin
administration interface (described below). By convention the
translation key is in lower case. All other translation keys are upper
case and include the group after the type code, in this case
PLG_CONTENT_J4XDEMOSTOC_usage.

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <extension type="plugin" version="4.0" method="upgrade" group="content">
        <name>plg_content_j4xdemostoc</name>
        <creationDate>August 2019</creationDate>
        <author>Clifford E Ford</author>
        <authorEmail>cliff@ford.myzen.co.uk</authorEmail>
        <authorUrl>http://www.fford.me.uk/</authorUrl>
        <copyright>Copyright (C) 2019 Clifford E Ford, All rights reserved.</copyright>
        <license>GNU/GPLv3 http://www.gnu.org/licenses/gpl-3.0.html</license>
        <!--  The version string is recorded in the components table -->
        <version>0.1.0</version>
        <!-- The description is optional and defaults to the name -->
        <description>PLG_CONTENT_J4XDEMOSTOC_XML_DESCRIPTION</description>
    
        <files>
            <filename plugin="j4xdemostoc">j4xdemostoc.php</filename>
        </files>
        
        <languages folder="administrator/language">
            <language tag="en-GB">language/en-GB/en-GB.plg_content_j4xdemostoc.ini</language>
            <language tag="en-GB">language/en-GB/en-GB.plg_content_j4xdemostoc.sys.ini</language>
        </languages>
    
        <config>
            <fields name="params">
                <fieldset name="basic">
                    <field
                        name="help"
                        type="text"
                        label="PLG_CONTENT_J4XDEMOSTOC_PARAMS_APPLIES_LABEL"
                        description="PLG_CONTENT_J4XDEMOSTOC_PARAMS_APPLIES_DESC"
                        default="PLG_CONTENT_J4XDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                        readonly="readonly"
                    />
    
                    <field 
                        name="toc_class"
                        type="list"
                        label="PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                        description="PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                        default="light"
                    >
                        <option value="light">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_LIGHT</option>
                        <option value="info">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_INFO</option>
                        <option value="primary">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_PRIMARY</option>
                        <option value="secondary">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_SECONDARY</option>
                        <option value="success">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_SUCCESS</option>
                        <option value="danger">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_DANGER</option>
                        <option value="warning">PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_WARNING</option>
                    </field>
    
                    <field 
                        name="toc_head_class"
                        type="list"
                        label="PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                        description="PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                        default="center"
                    >
                        <option value="center">center</option>
                        <option value="left">left</option>
                    </field>
    
                    <field 
                        name="list_indent"
                        type="list"
                        label="PLG_CONTENT_J4XDEMOSTOC_PARAMS_CONTENTS_INDENT_LABEL"
                        description="PLG_CONTENT_J4XDEMOSTOC_PARAMS_CONTENTS_INDENT_DESC"
                        default="1"
                    >
                        <option value="1">1rem</option>
                        <option value="2">2rem</option>
                        <option value="3">3rem</option>
                    </field>
    
                </fieldset>
            </fields>
        </config>
    </extension>
```

## The language Files

In case you were wondering, the ***sys.ini*** file is used by the system
during installation and when listing plugins. The ***.ini*** file is
used when displaying the plugin parameters form. And if any strings
appear in the site interface they can be located here too. That is
unusual but it provides for possible alternative titles, such as
*Contents* or *Index*. Those terms could be included in the placeholder
but would then be untranslatable.

The en-GB.plg_content_j4xdemostoc.sys.ini file:

```ini
    PLG_CONTENT_J4XDEMOSTOC="Content J4xdemos ToC"
    PLG_CONTENT_J4XDEMOSTOC_XML_DESCRIPTION="Render Article Table of Contents."
```

The en-GB.plg_content_j4xdemostoc.ini file:

```ini
    PLG_CONTENT_J4XDEMOSTOC_CONTENTS="Contents"

    PLG_CONTENT_J4XDEMOSTOC_PARAMS_APPLIES_LABEL="Applies to"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_APPLIES_DESC="Usage: {ToC} or {toc} in the article where the rable of contents is to appear."
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_APPLIES_DEFAULT="Articles"

    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_LABEL="Contents card class"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_DESC="For card and border colours"

    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_LIGHT="Bootstrap light: Light grey"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_INFO="Bootstrap info: Aquamarine"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_PRIMARY="Bootstrap primary: Blue"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_SECONDARY="Bootstrap secondary: Grey"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_SUCCESS="Bootstrap success: Green"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_DANGER="Bootstrap danger: Red"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_CLASS_OPTION_WARNING="Bootstrap Warning: Yellow"

    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL="Card heading class"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC="Should the word 'Contents' appear at the left or center?"

    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CONTENTS_INDENT_LABEL="List item indent"
    PLG_CONTENT_J4XDEMOSTOC_PARAMS_CONTENTS_INDENT_DESC="The distance to indent the content list links in rem units."
```

## The Plugin Administration Form

This form is available via the Administrator interface in System -\>
Manage -\> Plugins. Remember to enable the plugin for it to take effect.

<img src="https://docs.joomla.org/images/0/0e/J4xdemostoc-form.png"
decoding="async" data-file-width="495" data-file-height="613"
width="495" height="613" alt="The Parameters Form" />

The first item in the form, **Applies to**, is a reminder that plugin
works only with articles and not featured articles or modules. or
anything else.

The remaining items set the appearance of the Toc:

- **Contents card class** selects one of the Bootstrap card styles from
  a drop-down list.
- **Card heading class** selects left or center alignment of the header
  text.
- **List item indent** selects the relative amount of indentation of
  each item in the contents list.

Note that for accessibility reasons it is better to show the form item
descriptions in plain text rather than tooltips or popovers.

## The Plugin Code

Much of the plugin code is routine php for manipulating text. Please see
the code on github for the full listing. It is well commented. Here are
the significant lines from j4demostoc.php:

### The class statement

```php
        defined('_JEXEC') or die;
        
        use Joomla\CMS\Plugin\CMSPlugin;
        use Joomla\CMS\Language\Text;
        
        // See https://docs.joomla.org/J4.x:Creating_a_Plugin_for_Joomla
        
        /**
         * Create and or render an article table of contents.
         *
         * @since  4.0
         */
        class PlgContentJ4xdemostoc extends CMSPlugin
        {
```

Do not forget to start the code with the check for loading by Joomla or
die!

The **use** statements tell the loader which external classes are needed
by this plugin. **CMSPlugin** is the parent class of
PlgContentJ4xdemostoc and **Text** is used to translate the *List of
Contents* heading.

Notice the class statement: It must begin with **Plg** followed by the
plugin group, in this case **Content**, followed by the name of this
plugin **J4xdemostoc**.

The doc blocks are important too! They are used by automated
documentation tools.

### The entry point

```php
        /**
         * Look for {ToC} or {toc} and replace with Contents Panel.
         *
         * @return  void
         *
         * @since   4.0
         */
        public function onContentPrepare($context, &$article, &$params, $page = 0)
        {
```

**Important:** To invoke the entry point function a named event must
have been triggered. The onContentPrepare event is generated during
article creation so this plugin just needs to respond to that event. It
is not necessary, even wrong, to try to trigger that named event
anywhere else.

The \$context variable will contain a string such as
'com_content.article' or 'com_content.featured'. That can be tested to
if or how this plugin should respond.

The \$article variable contains an object passed by reference. Any
changes made to it in the plugin are set in the object. So the plugin
does not need to return any variables.

The \$params variable contains the article parameters.

### Testing what action to take

```php
            // featured article with readmore separator needs {ToC} removed
            if ($context === 'com_content.featured')
            {
                $article->text = str_ireplace('{ToC}', '', $article->text);
                return;
            }

            // the context could be something other than com_content
            // such as a module - in which case do nothing and return
            if ($context !== 'com_content.article')
            {
                return;
            }

            // return if there is no {ToC} in the article content
            if (stripos($article->text,'{ToC}') === false)
            {
                return;
            }

            // Load plugin language file only when needed
            $this->loadLanguage();
```

All of this is explained in the in-code comments so no further
explanation is needed here. The rest of the plugin code is text
manipulation: extracting the headings to create a list of contents with
links and modifying the headings to add anchors.

## Bugs

There are going to be bugs. Probably not for the author but probably so
for anyone else. So step through the code with the debugger and look at
the variables; use temporary var_dump() statements or echo and die
statements; and use the browser Developer Tools for display problems.
First time around I forgot to handle the {Toc} placeholder in featured
articles. And the Content card display was poor.

On the Bootstrap card display: the card background color is set to one
of the featured colors. The card header color is then set to black with
an opacity of 0.03. This is just about perceptible on the Bootstrap site
but not at all with the Cassiopeia template. An added in-line style
statement upping the header background opacity to 0.1 produced a much
improved distinction.

## Screenshot

This is the a cropped screenshot of the outcome for a tutorial article
written with Joomla 4.0 Alpha 10:

<img
src="https://docs.joomla.org/images/e/e4/J4xdemostoc-panel-light.png"
decoding="async" data-file-width="629" data-file-height="609"
width="629" height="609" alt="The rendered Contents Panel" />

The other Bootstrap colour schemes are ghastly with the Cassopeia
template.

Clifford E Ford August 2019
