<!-- Filename: J4.x:Triggering_content_plugins_in_your_extension / Display title: Triggering content plugins in your extension -->

Joomla!  4.x

A common example of [using
plugins](https://docs.joomla.org/Supporting_plugins_in_your_component "Special:MyLanguage/Supporting plugins in your component")
is to run the **content plugins** on some text. This is useful if you
want to support plugins that usually work on Content from a custom
extension. For the content prepare trigger you can simply call:

```php
    $text = \Joomla\CMS\HTML\HTMLHelper::_('content.prepare', $text);
```

For any other content triggers you must call:

```php
    // These 3 statements go at the top of your file directly below the JEXEC statement
    use Joomla\CMS\CMSObject;
    use Joomla\CMS\Factory;
    use Joomla\CMS\Plugin\PluginHelper;

    // This code goes in your component method that's triggering the plugins
    $article = new \stdClass;
    $article->text = $text;

    // add more to parameters if needed
    $params = new CMSObject;

    PluginHelper::importPlugin('content');
    $app = Factory::getApplication();
    $app->triggerEvent('onContentPrepare', array('some.context', &$article, &$params, 0));
```

You might want to look at core components (for example com_content) for
an example. See the [triggers
page](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
for the possible content plugin triggers.
