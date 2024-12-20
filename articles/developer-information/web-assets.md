<!-- Filename: J4.x:Web_Assets / Display title: Web Assets -->

## About Web Assets

There is a complete description of Web Assets in the Joomla! Programmers Documentation, reproduced in [this site](jdocmanual?article=docus/web-asset-manager/index) or available from the [original source](https://manual.joomla.org/docs/general-concepts/web-asset-manager/).

## Additional Examples

In Jdocmanual, many of the articles contain code blocks which benefit from syntax highlighting. Here is how it is accomplished:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```
This code is in the `default.php` file that is used to display this page! It is a little tricky to implement because the `hljs.highlightAll()` call must be made after the `css` and `js` files have been loaded and again whenever the page content is changed with a JavaScript call.

Other syntax highlighters are available!
