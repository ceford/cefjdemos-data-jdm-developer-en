<!-- Filename: J4.x:Namespace_Conventions_In_Joomla / Display title: Namespaces -->

## Introduction

The use of **PHP Namespaces** is a way to group related classes, interfaces, functions, and constants to avoid name conflicts in larger projects. They allow developers to organize code more effectively, especially when working with third-party libraries or large codebases that might have duplicate class names.

There is more information in the [Namespace](jdocmanual?article=docus/namespaces/index) section of the Joomla! Programmers Documentation.

### Key Features of PHP Namespaces

1. **Avoid Naming Conflicts**:
   - Without namespaces, if two classes have the same name, PHP will throw an error. Namespaces provide a context to uniquely identify classes or other elements.
2. **Organize Code**:
   - Namespaces allow logical grouping of related classes or functions, making the code easier to read and maintain.
3. **Access via Fully Qualified Names**:
   - Classes and functions within a namespace can be accessed using their fully qualified name, which includes the namespace path.

### Common Practices:
- Use namespaces to mirror your project folder structure for consistency.
- Import namespaces using the `use` keyword for cleaner code.

## The Manifest File

The initial declaration of an extension namespace is located in an extension manifest file, as in this example:

```xml
    <namespace path="src">Acme\Component\Wonderful</namespace>
```

The items in this declaration are as follows:

- **path="src"** tells the class loader that the namespaced classes are in the **src** folder of the extension, for example com_wonderful/src.
- **Acme** is the vendor prefix. For Joomla core extensions that would be *Joomla*. You need to invent your own unique vendor prefix.
- **Component** indicates the type of extension (component, module, plugin, template, library, ...).
- **Wonderful** is the extension name. Choose an extension name that is short, meaningful and likely to be unique.

## The Class File

The namespace declaration must be the first statement in the file in which it is used. For example:

```php
<?php

/**
 * @package     Wonderful
 * @subpackage  Site
 *
 * @copyright   (C) 2024 Merlin. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace Acme\Component\Wonderful\Controller;

use Joomla\CMS\MVC\Controller\BaseController;

// phpcs:disable PSR1.Files.SideEffects
\defined('_JEXEC') or die;
// phpcs:enable PSR1.Files.SideEffects

/**
 * Controller to display the default page - display of wonderful items
 *
 * @since  1.0.0
 */
class MagicController extends BaseController
{
    public function dosomething($wonderful)
    {
        // ... output something wonderful!
    }
}
```
## Using the Class File

When you need to use the class, for example in a template, you would insert a **use** statement at the top of the template:

```
use Acme\Component\Wonderful\Controller\MagicController;
```

And then instantiate the class and call the function:

```
$magic = new MagicController;
$result = $magic->dosomething('wonderful');
```

## The autoload_psr4.php File

This file is located in the administrator/cache folder of the Joomla site. It contains a complete map of the namespaces extracted from manifest files. It is generated on installation and regenerated whenever an extension is installed or reinstalled. You can delete this file and it will be regenerated on the next page load. It is sometime necessary to do this if extension installation has been attempted by an unusual method. The file contains 275 or more paths to namespaced class locations.

**PSR** is an acronym for [**PHP Standards Recommendation**](https://www.php-fig.org/psr/) and [**PSR-4: Autoloader**](https://www.php-fig.org/psr/psr-4/) is an accepted standard for loading PHP classes.
