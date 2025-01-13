<!-- Filename: J4.x:SCSS_and_Sass / Display title: Testing CSS and JavaScript -->

## Introduction

In a production Joomla site, Joomla delivers compressed CSS and Javascript files (those with `.min` in the filename). Servers will send the gzipped versions if they are available (those ending with a `.gz` suffix). These files are created from uncompressed sources. In the case of CSS files they are often created by processing SCSS precursors. For testing code that includes new or updated CSS or JavaScript it is necessary to rebuild the CSS files and the compressed and minified versions from the changed sources.

## Test Installation

For testing purposes you need a test server that has Git, Node.js and Composer installed. Go to the [Joomla CMS](https://github.com/joomla/joomla-cms) repository on GitHub and follow the instructions on *How to get a working installation from the source* near the bottom of the README text..

On completion of Joomla installation do not delete the Installation Directory! That will also delete the `build` directory that is needed for rebuilding CSS and JavaScript files.

The core `.scss` files are in the following directories:

- media/templates/site/cassiopeia/scss
- media/templates/administrator/atum/scss

The CSS generation script, SCSS compiler, and other similar build tools are located in the `build/` directory. The build directory is only available from the Joomlaǃ source, it is not included in an official Joomlaǃ release.

The installation instructions include the following:

```sh
git checkout 5.2-dev (or whatever branch you wish to work with)
composer install
npm ci
```

The `npm ci` command is a synonym for `npm clean-install` used in any situation where you want to make sure you're doing a clean install of your dependencies.

## Everyday use

There are alternative commands for use in situations when only SCSS or JavaScript files have been changed:

```sh
npm run build:css¶
    This command compiles SCSS files to CSS and also creates the minified files.

npm run build:js¶
    This command compiles and transpiles the JavaScript files to the correct format and creates minified files.
```

The commands crawl the media and templates directories and build the final file versions required by a Joomla installation.

## Sass, SCSS and CSS

> Sass is a stylesheet language that’s compiled to CSS. It allows you to use variables, nested rules, mixins, functions, and more, all with a fully CSS-compatible syntax. Sass helps keep large stylesheets well-organized and makes it easy to share design within and across projects. Sass files have the suffix `scss`.

### CSS

CSS code is expressed as followsː

```css
#header {
    margin: 0;
    border: 1px solid blue;
}
#header p {
    font-size: 14px;
    color: blue;
}
#header a {
    text-decoration: none;
}
```

### SCSS

`SCSS` is an extension of the syntax of CSS. It is used in Joomlaǃ core.

```css
$color:    blue;
#header {
    margin: 0;
    border: 1px solid $color;
    p {
        color: $colorRed;
        font: {
            size: 14px;
        }
    }
    a {
        text-decoration: none;
    }
}
```

An older `Sass` syntax provides a more concise way of writing CSS.

```css
$color:    blue
#header
    margin: 0
    border: 1px solid $color
        p
            color: $colorRed
            font:
                size: 14px
        a
            text-decoration: none
```

You can find more information in the [Sass Documentation](http://sass-lang.com/documentation/syntax/).

## From existing CSS to SCSS

If you want to customize the Cassiopeia template it is a good idea to copy the template and customize it afterwards so that Joomla! updates do not overwrite your customizations. To add your own CSS files to a copy of the Cassiopeia template just rename your `.css` files to `.scss`. You can then make use of the features SCSS has to offer:

- Language extensions such as variables, nesting, and mixins
- Many useful functions for manipulating colors and other values
- Advanced features like control directives for libraries
- Well-formatted, customizable output

See the [Sass site](https://sass-lang.com/) for full details.

### Import your .css as .scss files

Assuming you want to include your custom files and have them parsed by the SCSS compiler - you do NOT need to rewrite your .css to SCSS because plain `.css` works as well. What you have to do:

- rename your `.css` files to `.scss`
- prefix them with an `_`
- add an `@import` statement at the bottom of `media/templates/site/copy_of_cassiopeia/scss/template.scss` so that it overrides existing declarations:

```css
// Bootstrap functions
@import "../../../media/vendor/bootstrap/scss/functions";
//...
// 50 or more lines of import statements
// Finally add the cassiopeia colours
:root {
  @each $color, $value in $cassiopeia-colors {
    --#{$color}: #{$value};
  }
// More lines containing scss color statements, then your own styles:
@import "mystyles";
}
```

If you now compile your main template.scss file you end up with one main template.css optimized and minified. You also reduced your http requests and the site should load faster.
