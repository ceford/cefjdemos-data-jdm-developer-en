<!-- Filename: J4.x:SCSS_and_Sass / Display title: SCSS and Sass -->

Most of the Joomla 4.x default stylesheets are written using <a
href="https://en.wikipedia.org/wiki/https://en.wikipedia.org/wiki/Sass_(stylesheet_language))"
class="extiw"
title="wikipedia:https://en.wikipedia.org/wiki/Sass (stylesheet language))">SCSS</a>
and then compiled to generate the
<a href="https://en.wikipedia.org/wiki/CSS" class="extiw"
title="wikipedia:CSS">CSS</a> files.

## Where can you find the .scss stylesheets and compiler?

The core `.scss` files are in different directories. Specifically, these
are these filesː

- templates/cassiopeia/scss/template.scss
- administrator/templates/atum/scss/bootstrap.scss
- administrator/templates/atum/scss/font-awesome.scss
- administrator/templates/atum/scss/template.scss
- administrator/templates/atum/scss/template-rtl.scss
- media/plg_installer_webinstaller/scss/client.scss

The CSS generation script, SCSS compiler, and other similar build tools
are located in the `build/` directory of the Joomlaǃ source located on
<a href="https://github.com/joomla/joomla-cms" class="external text"
target="_blank" rel="nofollow noreferrer noopener">GitHub</a>. Refer to
[Git for
Coders](https://docs.joomla.org/Git_for_Coders "Special:MyLanguage/Git for Coders")
for more information on using GitHub. The build directory is only
available from the Joomlaǃ source, it is not included in an official
Joomlaǃ release.

## Prerequisites

Compiling your own SCSS files with Joomlaǃ core requires your local
machine to have Node.js installed in it. To install Node.js, please go
to the
<a href="https://nodejs.org/en/" class="external text" target="_blank"
rel="nofollow noreferrer noopener">Node.js</a> official web site,
download the respective setup for your operating system and install it
by following the installation wizard. NPM (Node Package Manager) is used
to manage and setup the JavaScript testing environment.

## Install dependencies (packages)

Open a command line and navigate to the root directory of Joomla 4.x.

Execute command `npm install`

```bash
    $ npm install
    ...
    added 1354 packages in 193.687s
    $
```

This will install all the dependencies to the `/node_modules` directory.
If a `/node_modules` folder does not exist, a folder will be
automatically created by npm. It is OK, if you see some warnings, but
you should not see an error message.

## How to re-generate the CSS style sheets

To re-generate all the CSS files from a Joomlaǃ core distribution, you
will need to execute the generation scripts as a
<a href="https://en.wikipedia.org/wiki/Command-line_interface"
class="extiw" title="wikipedia:Command-line interface">CLI
application</a>.

### All Joomlaǃ core scss files

You can compile all Joomlaǃ core css files with the command
`npm run build:css`. For exampleː

```bash
    $ npm run build:css
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.
    Prefixing for:  last 1 version
    template-rtl.css was updated.
    Prefixing for:  last 1 version
    font-awesome.css was updated.
    Prefixing for:  last 1 version
    client.css was updated.
    Prefixing for:  last 1 version
    bootstrap.css was updated.
    Prefixing for:  last 1 version
    template.css was updated.
    $
```

### All files in one directory

You can compile all files in one directory with the command
`node build --compile-css path-to-directory`. For exampleː
`node build --compile-css templates/cassiopeia/scss`.

```bash
    $ node build --compile-css templates/cassiopeia/scss
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.
```

### One special file

You can compile one file with the command
`node build --compile-css pathtofile`. For exampleː
`node build --compile-css templates/cassiopeia/scss/template.scss`.

```bash
    $ node build --compile-css templates/cassiopeia/scss/template.scss
    Prefixing for:  last 1 version
    Without `from` option PostCSS could generate wrong source map and will not find Browserslist config. Set it to CSS file path or to `undefined` to prevent this warning.
    template.css was updated.
```

Noteː

- The file extension .scss must be lowercase.
- The css file must be created.

## Compiling your own SCSS files for your template

To compile scss files for your own template, you will need to take a
copy of the `JOOMLA4/build/build-modules-js/compilecescss.js` script and
adjust it to suit your environment.

Alternatively you can use a SCSS Compiler plugin which compiles your
`.scss` files automatically on page reload. In the Joomlaǃ Extension
Directory JED you will find a plugin for this purposeː <a
href="https://extensions.joomla.org/extensions/extension/?searchall=scss&amp;filter%5Btags%5D%5B%5D=&amp;filter%5Bcore_catid%5D=&amp;filter%5Bincludes%5D=&amp;filter%5Bversions%5D=&amp;filter%5Btype%5D=&amp;filter%5Bhasdemo%5D=&amp;filter%5Bnewupdated%5D=&amp;filter%5Bscore%5D=&amp;dir=DESC&amp;limitstart=&amp;controller=filter&amp;view=extension&amp;layout=list&amp;Itemid=145&amp;clearorders=0&amp;clearfilters=1"
class="external text" target="_blank" rel="noreferrer noopener">Joomlaǃ
Extension Directory</a>

More alternatives and tools you can find on
<a href="http://sass-lang.com/install" class="external free"
target="_blank"
rel="nofollow noreferrer noopener">http://sass-lang.com/install</a>

The SCSS compiler used for the Joomlaǃ 4.x core is
<a href="https://github.com/sass/node-sass" class="external text"
target="_blank" rel="nofollow noreferrer noopener">node-sass</a>. In the
Joomlaǃ core you can find it in the directory
`JOOMLA4/node_modules/node-sass`.

## Difference CSS, SCSS and Sass

`SCSS` or `Sass` simplifies CSS notation and defines variables. This
ensures a clearer presentation, less code and a simpler revision through
the use of variables.

### CSS

With CSS we write code like below in full lengthː

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

### Sass

Sass is a CSS preprocessor with syntax improvements. Style sheets in the
extended syntax are processed by the preprocessor and converted into
regular CSS style sheets.

There are two syntaxes available for Sassː `SCSS` and `Sass`.

#### SCSS

`SCSS` is used in Joomlaǃ core. `SCSS` is an extension of the syntax of
CSS.

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

#### Sass

The older `Sass` syntax provides a more concise way of writing CSS.

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

You can find more information in the <a
href="http://sass-lang.com/documentation/file.SASS_REFERENCE.html#syntax"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">Sass Documentation</a>.

## From existing CSS to SCSS / import CSS files

You may want to add your existing CSS files and classes to your SCSS
powered Cassiopeia template or start with what you have. All CSS
declarations are compatible with SCSS so you can just rename your `.css`
files to `.scss` and you can compile and use them. You can then step by
step make use of the features SCSS has to offer:

- Language extensions such as variables, nesting, and mixins
- Many useful functions for manipulating colors and other values
- Advanced features like control directives for libraries
- Well-formatted, customizable output

See
<a href="https://sass-lang.com/" class="external free" target="_blank"
rel="nofollow noreferrer noopener">https://sass-lang.com/</a>

Notice: If you want to customize Cassiopeia template it is a good idea
to copy the template and customize it afterwards - so that Joomla!
updates do not overwrite your customizations.

### Import your .css as .scss files

Now lets assume you want to include your custom files and get them
parsed by SCSS compiler - you do NOT need to rewrite your .css to SCSS
because plain `.css` works as well. The only thing you have to do is

- to rename your `.css` files to `.scss` and
- prefix it with an `_` and
- add an `@import` statement into above mentioned main
  `templates/cassiopeia/scss/template.scss`

I assume you put your custom `mystyles.css` file renamed to
`_mystyles.scss` into the `/templates/cassiopeia/scss` folder. Now you
open `/templates/cassiopeia/scss/template.scss` and at the bottom of the
file your `_mystyles.scss` file so that it overrides existing
declarations:

```css
    // Bootstrap functions
    @import "../../../media/vendor/bootstrap/scss/functions";


    // Variables
    @import "variables";

    // Flying Focus
    @import "../../../media/vendor/flying-focus-a11y/scss/flying-focus";

    // Bootstrap
    @import "../../../media/vendor/bootstrap/scss/variables";
    @import "../../../media/vendor/bootstrap/scss/bootstrap";

    // FontAwesome
    @import "../../../media/vendor/font-awesome/scss/font-awesome";

    // B/C for Icomoon
    @import "../../../media/system/scss/icomoon";

    // Alert
    @import "../../../media/system/scss/jalert";

    // Blocks
    @import "blocks/global"; // Leave this first
    @import "blocks/alerts";
    @import "blocks/banner";
    @import "blocks/demo-styling";
    @import "blocks/footer";
    @import "blocks/form";
    @import "blocks/frontend-edit";
    @import "blocks/header";
    @import "blocks/icons";
    @import "blocks/iframe";
    @import "blocks/layout";
    @import "blocks/modals";
    @import "blocks/modifiers";
    @import "blocks/utilities";
    @import "blocks/css-grid"; // Last to allow fallback

    // Vendor overrides
    @import "vendor/awesomplete";
    @import "vendor/bootstrap/buttons";
    @import "vendor/bootstrap/card";
    @import "vendor/bootstrap/custom-forms";
    @import "vendor/bootstrap/collapse";
    @import "vendor/bootstrap/dropdown";
    @import "vendor/bootstrap/lists";
    @import "vendor/bootstrap/modal";
    @import "vendor/bootstrap/nav";
    @import "vendor/bootstrap/pagination";
    @import "vendor/bootstrap/table";
    @import "vendor/chosen";
    @import "vendor/dragula";
    @import "vendor/minicolors";
    @import "vendor/tinymce";

    @import "mystyles";
```

If you now compile your main template.scss file you end up with one main
template.css optimized and minified. You also reduced your http requests
and the site should load faster.
