<!-- Filename: J4.x:J4_Plugin_example_-_Table_of_Contents / Display title: Example: Table of Contents -->

## Introduction

This article provides an example of a content plugin to illustrate coding using the latest Joomla coding conventions. The complete code is available from [GitHub](https://github.com/ceford/cefjdemos-plg-content-toc). You can download and install it or just unpack it to inspect the code.

There is a section on [Plugin Development](https://manual.joomla.org/docs/building-extensions/plugins/) and more examples in the Joomla Programmers Documentation, [copied to this site](jdocmanual?article=docus/plugins/index) for convenience.

This plugin generates a *Table of Contents* from the headings in any article containing a `{cefjdemostoc}` placeholder. The result is illustrated in the last screenshot in this article.

## Source Code Structure

The following is a schematic illustration of the structure of the source code used to create the extension:

```
cefjdemos-plg-toc
|-- .vscode (folder for build settings)
|-- plg_content_cefjdemostoc (folder compressed to create an installable zip file)
    |-- language/en-GB/ (language folder, kept with the extension code)
        |-- plg_content_cefjdemostoc.ini
        |-- plg_content_cefjdemostoc.sys.ini
    |-- media
        |-- css
            |-- cefjdemostoc.css (layout styles)
    |-- services (folder for dependency injection)
        |-- provider.php (the DI code)
    |-- src (folder for namespaced classes)
        |-- Extension (folder for extension specific code)
            |-- Cefjdemostoc.php (the extension class code)
    |-- tmpl (folder for layouts)
        |-- default.php (the table of contents layout)
    |-- cefjdemostoc.xml (the manifest file)
|-- .gitignore (items not to include in the git repository)
|-- build.xml (instructions for building the extension with phing)
|-- LICENSE (the license statement)
|-- README.md (brief description and instructions on use)
```

This is the structure as seen in the VSCode or VSCodium IDE:

![Plugin development file structure in vscodium](../../../en/images/plugins/cefjdemostoc-vscodium.png)

## The Manifest File

Every extension must have an xml format manifest file. It is used by Joomla for installation, update and removal purposes. This is the manifest file for this extension:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="plugin" group="content" method="upgrade">
    <name>plg_content_cefjdemostoc</name>
    <author>Clifford E Ford</author>
    <creationDate>2024-12-23</creationDate>
    <copyright>(C) 2024 Clifford E Ford</copyright>
    <license>GNU General Public License version 2 or later; see LICENSE.txt</license>
    <authorEmail>cliff@xxxx.yyyy.co.uk</authorEmail>
    <authorUrl>jdocmanual.org</authorUrl>
    <version>0.2.2</version>
    <description>PLG_CONTENT_CEFJDEMOSTOC_XML_DESCRIPTION</description>
    <namespace path="src">Cefjdemos\Plugin\Content\Cefjdemostoc</namespace>
    <files>
        <folder plugin="cefjdemostoc">services</folder>
        <folder>src</folder>
        <folder>language</folder>
    </files>
    <media destination="plg_content_cefjdemostoc" folder="media">
        <folder>css</folder>
    </media>
    <changelogurl>https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/changelog.xml</changelogurl>
    <updateservers>
        <!-- Note: No spaces or linebreaks allowed between the server tags -->
        <server type="extension" priority="2" name="Cefjdemostoc Update Site">https://raw.githubusercontent.com/ceford/cefjdemos-plg-content-toc/main/updates.xml</server>
    </updateservers>
    <config>
        <fields name="params">
            <fieldset name="basic">
                <field
                    name="help"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DESC"
                    default="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT"
                >
                    <option value="">PLG_CONTENT_CEFJDEMOSTOC_PARAMS_APPLIES_DEFAULT</option>
                </field>

                <field
                    name="toc_class"
                    type="text"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_CLASS_DESC"
                />

                <field
                    name="toc_head_class"
                    type="list"
                    label="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_LABEL"
                    description="PLG_CONTENT_CEFJDEMOSTOC_PARAMS_CARD_HEAD_CLASS_DESC"
                    default="center"
                >
                    <option value="text-center">text-center</option>
                    <option value="text-start">text-start</option>
                </field>
            </fieldset>
        </fields>
    </config>
</extension>
```

## The services/provider.php file

The purpose of this file is to register a service provider and declare any tools it needs. This particular plugin does not need much. If your custom plugin needed to make database queries you would declare it here. Have a look at an authentication plugin. They require database and user tools.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjdemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford <https://jdocmanual.org>
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\Extension\PluginInterface;
use Joomla\CMS\Factory;
use Joomla\CMS\Plugin\PluginHelper;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;
use Joomla\Event\DispatcherInterface;
use Cefjdemos\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc;

return new class () implements ServiceProviderInterface {
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.3.0
     */
    public function register(Container $container)
    {
        $container->set(
            PluginInterface::class,
            function (Container $container) {
                $plugin     = new Cefjdemostoc(
                    $container->get(DispatcherInterface::class),
                    (array) PluginHelper::getPlugin('content', 'cefjdemostoc')
                );
                $plugin->setApplication(Factory::getApplication());

                return $plugin;
            }
        );
    }
};
```

## The Extension Class

The purpose of the src/Extension/Cefjdemostoc.php file is to prepare data for output. The file is 91 lines long so is not reproduced here. However, some parts of it warrant further explanation.

### Codesniffer Directives

Lines 16 to 18 have this:

```php
// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects
```

The commented lines avoid error reports from the CodeSniffer. They serve no other purposes.

### public function onContentPrepare()

This is the event that the plugin responds to. It is passed a *context*, the article data and the article parameters. A *Table of Contents* should be generated only if the rendered page is a single article. Otherwise, the `{cefjdemostoc}` placeholder needs to be removed.

To generate a table of contents, some article parameters are set and each heading in the article is altered to to add a sequence number **id**, so `<h2>Introduction</h2>` becomes `<h2 id="cefjemostoc0">Introduction</h2>`. A template is then loaded to add the rendered table of contents.

### The tmpl/default.php File

The purpose of this file is to render the output with the data provided. The file can be overridden and you will see it amongst the **plugins/contents** overrides for the Cassiopeia template.

The file is reproduced below. Note that some elements have hard coded CSS classes and some obtained from the article parameters. The **fs-** classes are Bootstrap font sizes. The headings are stored in the `$headings2` array.

```php
<?php

/**
 * @package     Cefjdemostoc.Plugin
 * @subpackage  Content.cefjemostoc
 *
 * @copyright   (C) 2024 Clifford E Ford.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;

/**
 * @var Joomla\CMS\WebAsset\WebAssetManager $wa
 * @var \Joomla\Plugin\Content\Cefjdemostoc\Extension\Cefjdemostoc $this
 */
$wa = $this->getApplication()->getDocument()->getWebAssetManager();
$wa->registerAndUseStyle('plg_content_cefjdemostoc', 'plg_content_cefjdemostoc/cefjdemostoc.css');

?>
<div class="card cefjdemostoc <?php echo $toc_class; ?>">
    <div class="card-header">
        <div class="<?php echo $toc_head_class; ?> fs-2">
            <?php echo Text::_('PLG_CONTENT_CEFJDEMOSTOC_CONTENTS'); ?>
        </div>
    </div>
    <div class="card-body">
        <ul class="list-group">
            <?php foreach ($headings2 as $i => $heading) : ?>
                <li class="list-group-item fs-<?php echo $heading[1] + 2; ?> ps-<?php echo $heading[1] + 2; ?>">
                    <a href="#cefjemostoc<?php echo $i; ?>" class="link-underline-light">
                        <?php echo $heading[2]; ?>
                    </a>
                </li>
            <?php endforeach; ?>
        </ul>
    </div>
</div>
```

#### The Web Asset Manager

Without additional styling, the table of contents (ToC) would occupy the full width of the screen at the location of the `{cefjemostoc}` placeholder in the source text. That is sometimes seen in technical publications but may not be what you want. On wide screens it is often better to float the ToC and allow tha article text to flow around it. However, that can become unusable on narrow screens. The best solution is to provide a style sheet with custom media queries. On wide screens the ToC is floated to the right but on narrow screens it is not.

The `default.php` code above shows how to include a custom style sheet for the plugin. The CSS is in the file `media/css/cefjdemostoc.css`. The values there are for illustration of this tutorial.

## Result

![The resulting table of contents](../../../en/images/plugins/cefjdemostoc-result.png)
