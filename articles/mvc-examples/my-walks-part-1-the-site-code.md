<!-- Filename: J4.x:My_Walks_Part_1:_The_Site_code / Display title: My Walks Part 1: The Site code -->

## J4 Component Tutorial: Mywalks Part 1 - The Site Code

## Background

As an experienced Joomla! 3.x developer I needed to learn about Joomla!
4.x and found it really difficult to get started. Although
*experienced*, my actual knowledge of Joomla internals is limited. I
started at the 1.6 stage and many new features passed me by. So this
tutorial may be a case of the blind leading the blind. It took me about
10 days to get something to work by reading the code, stepping through
with the debugger and reading the limited amount of J4 documentation
available. The tutorial started with Joomla 4.x at the Alpha 10 stage
and was updated for the Beta 4 stage.

**Update:** with the release of Joomla 4.3.1 in May 2023 I have come
back to update the Mywalks code, fix some bugs and add some new
features. Keeping the tutorial in sync with the code is an ongoing
problem. If in doubt, read the code!

## Component Purpose and Data Schema

For the last few years I have been going out for walks with my family,
sometimes once a week, sometimes twice, but only in good weather. I kept
a list - there are 50 or so walks and we have done each of them several
times. This was all part of trying to keep fit in old age. So for
self-teaching purposes I decided to develop a Joomla! component that has
two Site views: the list of walks and the details on individual walks.
To keep it simple I don't want any frills: no site side data entry, no
scores, hit counters, categories or other Joomla goodies. For test
purposes sample data was entered directly into the database with
phpMyAdmin. The component needs two database tables: the list of walks
and the list of individual visits. I decided to name the component
com_mywalks and the tables \#\_\_mywalks and \#\_\_mywalks_dates.

All of the code for this tutorial component can be obtained from here:

- [<https://github.com/ceford/j4xdemos-com-mywalks/archive/refs/heads/master.zip>](https://github.com/ceford/j4xdemos-com-mywalks/archive/refs/heads/master.zip)

If you go to <https://github.com/ceford/j4xdemos-com-mywalks>, Have a
look at the README.md file. Select the green **Code** button and then
the **Download ZIP** item at the foot of the drop-down menu.

You might find it helpful to install the component or unpack it without
installation and look at the working files. On first installation the
success messages says COM_MYWALKS_XML_DESCRIPTION. This is an installer
problem and is nothing to worry about. Just select Mywalks item from the
Administrator Components menu to see the sample data List of Walks.

### The mywalks table

During installation, the following sql statements create the tables used
by the Mywalks component:

``` sql
CREATE TABLE IF NOT EXISTS `#__mywalks` (
  `id` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `title` varchar(64) NOT NULL,
  `description` text NOT NULL,
  `distance` decimal(10,0) NOT NULL,
  `toilets` tinyint(1) NOT NULL DEFAULT '0',
  `cafe` tinyint(1) NOT NULL DEFAULT '0',
  `hills` int(11) NOT NULL DEFAULT '0',
  `bogs` int(11) NOT NULL DEFAULT '0',
  `picture` varchar(128) DEFAULT NULL,
  `width` int(11) DEFAULT NULL,
  `height` int(11) DEFAULT NULL,
  `alt` varchar(64) DEFAULT NULL,
  `state` TINYINT NOT NULL DEFAULT '1'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 DEFAULT COLLATE utf8mb4_unicode_ci;
```

The sql files in the working example zip file also include some sample
data.

### The mywalks_dates table

``` sql
CREATE TABLE IF NOT EXISTS `#__mywalk_dates` (
  `id` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  `walk_id` int(11) NOT NULL,
  `date` date NOT NULL,
  `weather` varchar(256) DEFAULT NULL,
  `state` TINYINT NOT NULL DEFAULT '1',
  KEY `idx_walk` (`walk_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 DEFAULT COLLATE utf8mb4_unicode_ci;
```

If this were a real component it would be obvious that the schema design
has a long way to go! Look at the WalkHighlands site to see how far.
However, it is enough for tutorial purposes.

## The Manifest file and component folder structure

The component zip file used for installation should contain the manifest
file named mywalks.xml (notice no com\_) along with admin and site
folders, like so:

    com_mywalks.zip
         admin
         site
         mywalks.xml

On installation the manifest file is copied to the
site_root/administrator/components/com_mywalks folder where it is needed
for uninstall purposes. It should not be there in the source code!
Entries are also made in site_root/administrator/cache/autoload_psr4.php
\[**New in Joomla 4**\].

### The manifest file

Note that the method is set to **upgrade** so the component can be
installed repeatedly, for example if the code is updated. However, the
sql statements will not be executed a second time. If the install sql
statements are not executed for any reason, try executing them manually
by copying them from the source code into phpMyAdmin.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<extension type="component" method="upgrade">
    <name>com_mywalks</name>
    <!-- The following elements are optional and free of formatting conttraints -->
    <creationDate>August 2019</creationDate>
    <author>Clifford E Ford</author>
    <authorEmail>cliff@ford.myzen.co.uk</authorEmail>
    <authorUrl>http://www.fford.me.uk/</authorUrl>
    <copyright>Copyright (C) 2019-2023 Clifford E Ford, All rights reserved.</copyright>
    <license>GNU/GPL Version 2 or later - http://www.gnu.org/licenses/gpl-2.0.html</license>
    <!--  The version string is recorded in the components table -->
    <version>0.3.0</version>
    <!-- The description is optional and defaults to the name -->
    <description>COM_MYWALKS_XML_DESCRIPTION</description>
    <namespace path="src">J4xdemos\Component\Mywalks</namespace>

    <install> <!-- Runs on install -->
        <sql>
            <file driver="mysql" charset="utf8">sql/install.mysql.sql</file>
        </sql>
    </install>
    <uninstall> <!-- Runs on uninstall -->
        <sql>
            <file driver="mysql" charset="utf8">sql/uninstall.mysql.sql</file>
        </sql>
    </uninstall>

    <!-- Site Main File Copy Section -->
    <!-- Note the folder attribute: This attribute describes the folder
        to copy FROM in the package to install therefore files copied
        in this section are copied from /site/ in the package -->

    <files folder="site">
        <folder>forms</folder>
        <folder>language</folder>
        <folder>src</folder>
        <folder>tmpl</folder>
    </files>
    
    <administration>
        <files folder="admin">
            <file>access.xml</file>
            <file>config.xml</file>
            <folder>forms</folder>
            <folder>language</folder>
            <folder>services</folder>
            <folder>sql</folder>
            <folder>src</folder>
            <folder>tmpl</folder>
        </files>
        <menu img="class:default" link="option=com_mywalks">com_mywalks</menu>
    </administration>
</extension>
```

### The Namespace

Notice the **namespace** tag in the manifest file. The first item should
be the Company name. I don't have one so I have used J4xdemos. The
namespace is used in the extension to distinguish its code from code in
other extensions that may have identical class names. The namespace is
used to register a service provider - see below.

The second item is the type of extension: Component, Module, Plugin or
Template. The third item is the extension name without preceding com\_,
mod\_, etc., Mywalks in this case.

The namespace attribute, path="src", indicates that all of the files
containing namespaced code will be found in the src directory.

## Language files

In case you are not familiar with Joomla extensions, the source site
language folder contains one file: en-GB.com_mywalks.ini, which contains
the translated values of fixed strings, used to allow translation from
English into other languages. The folder structure is simple:

    site                               - the folder containing the site files       
         language                      - the folder containing the site language translation file
              en-GB                    - the folder containing English translations
                   com_mywalks.ini     - the file of translated keys

And the com_mywalks.ini contains this:

    COM_MYWALKS_ERROR_WALK_NOT_FOUND="Walk not found!"
    COM_MYWALKS_LIST_PAGE_HEADING="List of Walks"
    COM_MYWALKS_LIST_TITLE="Title"
    COM_MYWALKS_LIST_DESCRIPTION="Description"
    COM_MYWALKS_LIST_DISTANCE="Distance in Km"
    COM_MYWALKS_LIST_LAST_VISIT="Last Visit"
    COM_MYWALKS_LIST_NVISITS="nVisits"
    COM_MYWALKS_LIST_TABLE_CAPTION="List of Walks"
    COM_MYWALKS_MYWALKS_FILTER_SEARCH_TITLE_DESC="Search in Title"
    COM_MYWALKS_WALK_BACK_TO_WALKS="Back to list of walks"
    COM_MYWALKS_WALK_REPORTS="Walk Reports"
    COM_MYWALKS_WALK_DATE="Visit date"
    COM_MYWALKS_WALK_WEATHER="Weather Report"

For each line, the first part is a **key** and the second part is its
**value**, the English translation. Any fixed text in the component site
interface should be in this file. For example, the walks list column
headings should be keys in the source code and translated here. Note
also that the primary language of Joomla is British English. Other
languages need separate translation files. By convention, the keys
should be sorted into alphabet order!

**Important:** Most Joomla language files are located in a common
language folder, for example site_root/language/en_GB/com_example.ini or
site_root/administrator/language/en_GB/com_example.ini. However, third
party extensions should place their language folders within the
component folder, for example
site_root/components/com_example/language/en_GB/com_example.ini and
similarly for the component administrator language files. This avoids
inadvertent removal of a language folder supplied with a third part
component should an administrator remove an installed language.

## The Site Files

You may notice that some of the Joomla! 4.x folder and file names begin
with Upper Case letters and others begin with Lower case letters. J4
also has a different structure from J3. In short, files with Upper Case
leading character names are name-spaced and those that are not are not.

Also, be aware that the code required to display site views also
includes some functions from the admin code, covered below.

### The tmpl files

The tmpl files contain the code that displays the page views. They ought
to be the easiest to explain and to understand. The tmpl file structure
in the source code looks like this:

    site
         tmpl
              mywalk
                  default.php
              mywalks
                  default.php
                  default.xml

#### The single walk view - tmpl/mywalk/default.php:

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Language\Text;
use Joomla\CMS\Router\Route;

?>
<div class="page-header">
    <h1><?php echo $this->item->title; ?></h1>
</div>

<p><?php echo $this->item->description; ?>!</p>

<h2><?php echo Text::_('COM_MYWALKS_WALK_REPORTS'); ?></h2>

<div class="table-responsive">
  <table class="table table-striped">
  <thead>
    <tr>
        <th scope="col"><?php echo Text::_('COM_MYWALKS_WALK_DATE'); ?></th>
        <th scope="col"><?php echo Text::_('COM_MYWALKS_WALK_WEATHER'); ?></th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->reports as $id => $report) : ?>
    <tr>
        <td><?php echo $report->date; ?></td>
        <td><?php echo $report->weather; ?></td>
    </tr>
    <?php endforeach; ?><?php //endif; ?>
    </tbody>
  </table>
</div>

<a href="<?php echo Route::_('index.php?option=com_mywalks'); ?>"><?php echo Text::_('COM_MYWALKS_WALK_BACK_TO_WALKS'); ?></a>
```

For newcomers to Joomla: each php file starts with a DocBlock used in
automated documentation; in namespaced files the next statement is the
namspace, not used in tmpl files; the first executable statement is
always `defined('_JEXEC') or die;` which ensures the file has been
loaded by Joomla and not called directly through a web url.

The `use Joomla\CMS\Language\Text` statement defines the location of the
class that converts string keys to string values. The class is loaded
when it is first needed, in this case via the `Text::_()` function call.
The `use` statements are almost always placed immediately below the
`defined('_JEXEC')` statement.

The remaining lines output the walk title, description and list of
visits extracted from the database.

#### The walks list view - tmpl/mywalks/default.php

This file outputs a page heading and then the list of walks.

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;
use J4xdemos\Component\Mywalks\Site\Helper\RouteHelper as MywalksHelperRoute;

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>

<h1><?php echo Text::_('COM_MYWALKS_LIST_PAGE_HEADING'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_mywalks'); ?>" method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
  <table class="table table-striped">
  <thead>
    <tr>
        <th scope="col">
            <?php echo HTMLHelper::_('searchtools.sort', 'JGLOBAL_TITLE', 'a.title', $listDirn, $listOrder); ?>
        </th>
        <th scope="col"><?php echo Text::_('COM_MYWALKS_LIST_DESCRIPTION'); ?></th>
        <th scope="col">
            <?php echo HTMLHelper::_('searchtools.sort', 'COM_MYWALKS_LIST_DISTANCE', 'a.distance', $listDirn, $listOrder); ?>
        </th>
        <th scope="col">
            <?php echo Text::_('COM_MYWALKS_LIST_LAST_VISIT'); ?>
        </th>
        <th scope="col">
            <?php echo Text::_('COM_MYWALKS_LIST_NVISITS'); ?>
        </th>
    </tr>
    </thead>
    <tbody>
    <?php foreach ($this->items as $id => $item) :
        $slug = preg_replace('/[^a-z\d]/i', '-', $item->title);
        $slug = strtolower(str_replace(' ', '-', $slug));
    ?>
    <tr>
        <td><a href="<?php echo Route::_(MywalksHelperRoute::getWalkRoute($item->id, $slug)); ?>">
        <?php echo $item->title; ?></a></td>
        <td><?php echo $item->description; ?></td>
        <td><?php echo $item->distance; ?></td>
        <td><?php echo $item->last_visit //$item->lastvisit; ?></td>
        <td><?php echo $item->nvisits; ?></td>
    </tr>
    <?php endforeach; ?><?php //endif; ?>
    </tbody>
  </table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

**Notes**

- The `use` statements define the locations of additional php files
  using their namespaces.
  - `Joomla\CMS\HTML\HTMLHelper` is for files used in page display, for
    example the Javascript files needed for table sorting.
  - `Joomla\CMS\Layout\LayoutHelper` is used to produce any one of a
    number of Joomla widgets.
- `$this->items` is an array containing the list of walks.
- A slug is created by converting the title to lower case alpha-numeric
  only with spaces replaced by minus signs. It is used to pass a
  descriptive term in the url of the link to the walk visits list as an
  aid to search engine optimisation.
- The static call to Route is used to create a url for a link to an
  individual walk description.

This is an extract from the getWalkRoute function:

``` php
public static function getWalkRoute($id, $slug, $language = 0, $layout = null)
    {
        // Create the link
        $link = 'index.php?option=com_mywalks&view=mywalk&id=' . $id . '&slug=' . $slug;

        if ($language && $language !== '*' && Multilanguage::isEnabled())
        {
            $link .= '&lang=' . $language;
        }

        if ($layout)
        {
            $link .= '&layout=' . $layout;
        }

        return $link;
    }
```

### Getting the data - The HtmlView files

The tmpl files are supposed to deal solely with the composition of html.
Any data needed to create the html, such as the list of walks, should be
stored in variables in the HtmlView files where they are made available
in the `$this` object.

#### The HtmlView.php file for the single walk view

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\View\Mywalk;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\View\GenericDataException;
use Joomla\CMS\MVC\View\HtmlView as BaseHtmlView;

/**
 * HTML Mywalk View class for the Mywalks component
 *
 * @since  1.5
 */
class HtmlView extends BaseHtmlView
{
    /**
     * The item model state
     *
     * @var    \Joomla\Registry\Registry
     * @since  1.6
     */
    protected $state;

    /**
     * The item object details
     *
     * @var    \JObject
     * @since  1.6
     */
    protected $item;

    /**
     * The list of visit reports/visit dates for this walk
     *
     * @var    \JObject
     * @since  1.6
     */
    protected $reports;

    /**
     * Execute and display a template script.
     *
     * @param   string  $tpl  The name of the template file to parse; automatically searches through the template paths.
     *
     * @return  mixed  A string if successful, otherwise an Error object.
     */
    public function display($tpl = null)
    {
        $this->state      = $this->get('State');
        $this->item       = $this->get('Item');
        $this->reports    = $this->get('Reports');

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        return parent::display($tpl);
    }
}
```

The display function is very simple. It fetches state, single walk and
walk report data for that walk from the model. If any of the data
retrieval steps return an error it throws an exception, usually
resulting in some sort of error message page. Otherwise control is
passed via Joomla to the tmpl file to create the html output. HtmlView
files can become quite complex - have a look at the content component
article HtmlView for for an example.

#### The HtmlView file for the list of walks

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\View\Mywalks;

defined('_JEXEC') or die;

use Joomla\CMS\Factory;
use Joomla\CMS\MVC\View\GenericDataException;
use Joomla\CMS\MVC\View\HtmlView as BaseHtmlView;

/**
 * Walks List View class
 *
 * @since  1.6
 */
class HtmlView extends BaseHtmlView
{
    /**
     * The item model state
     *
     * @var    \Joomla\Registry\Registry
     * @since  1.6.0
     */
    protected $state;

    /**
     * The item details
     *
     * @var    \JObject
     * @since  1.6.0
     */
    protected $items;

    /**
     * The pagination object
     *
     * @var    \JPagination
     * @since  1.6.0
     */
    protected $pagination;

    /**
     * The page parameters
     *
     * @var    \Joomla\Registry\Registry|null
     * @since  4.0.0
     */
    protected $params = null;

    /**
     * Method to display the view.
     *
     * @param   string  $tpl  The name of the template file to parse; automatically searches through the template paths.

     * @return  mixed  \Exception on failure, void on success.
     *
     * @since   1.6
     */
    public function display($tpl = null)
    {
        // Get data from the model.
        $this->state      = $this->get('State');
        $this->items      = $this->get('Items');
        $this->filterForm    = $this->get('FilterForm');
        $this->activeFilters = $this->get('ActiveFilters');
        $this->pagination = $this->get('Pagination');
        // Flag indicates to not add limitstart=0 to URL
        $this->pagination->hideEmptyLimitstart = true;

        // Check for errors.
        if (count($errors = $this->get('Errors')))
        {
            throw new GenericDataException(implode("\n", $errors), 500);
        }

        return parent::display($tpl);
    }
}
```

Ready for the models?

### Getting the data - The Model files

For the single walk model we need a model file that implements
populateState, getItem and getVisits. For the list of walks we need
populateState, getListQuery, getItems and a few more to sort on columns
and paginate long lists, neither of which are implemented in this
tutorial.

#### Model file: MywalkModel

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\Model;

defined('_JEXEC') or die;

use Joomla\Database\ParameterType;
use Joomla\CMS\Factory;
use Joomla\CMS\Language\Text;
use Joomla\CMS\MVC\Model\ItemModel;

/**
 * Mywalk Component Mywalk Model
 *
 * @since  1.5
 */
class MywalkModel extends ItemModel
{
    /**
     * Model context string.
     *
     * @var        string
     */
    protected $_context = 'com_mywalks.mywalk';

    /**
     * Method to auto-populate the model state.
     *
     * Note. Calling getState in this method will result in recursion.
     *
     * @since   1.6
     *
     * @return void
     */
    protected function populateState()
    {
        $app = Factory::getApplication();

        // Load state from the request.
        $pk = $app->input->getInt('id');
        $this->setState('mywalk.id', $pk);
    }

    /**
     * Method to get walk data.
     *
     * @param   integer  $pk  The id of the walk.
     *
     * @return  object|boolean  Menu item data object on success, boolean false
     */
    public function getItem($pk = null)
    {
        $pk = (!empty($pk)) ? $pk : (int) $this->getState('mywalk.id');

            try
            {
                $db = $this->getDatabase();
                $query = $db->getQuery(true)
                    ->select(
                        $this->getState(
                            'item.select', 'a.*'
                        )
                    );
                $query->from($db->quoteName('#__mywalks') . ' AS a')
                    ->where($db->quoteName('a.id') . ' = :id')
                    ->bind(':id', $pk, ParameterType::INTEGER);

                $db->setQuery($query);

                $data = $db->loadObject();

                if (empty($data))
                {
                    throw new \Exception(Text::_('COM_MYWALKS_ERROR_WALK_NOT_FOUND'), 404);
                }
            }
            catch (\Exception $e)
            {
                if ($e->getCode() == 404)
                {
                    // Need to go through the error handler to allow Redirect to work.
                    throw new \Exception($e->getMessage(), 404);
                }
                else
                {
                    $this->setError($e);
                    $this->_item[$pk] = false;
                }
            }

        return $data;
    }
    /**
     * Method to get walk visit data.
     *
     * @param   integer  $pk  The id of the walk.
     *
     * @return  object|boolean  Menu item data object on success, boolean false
     */
    public function getReports($pk = null)
    {
        $pk = (!empty($pk)) ? $pk : (int) $this->getState('mywalk.id');

        try
        {
            $db = $this->getDatabase();
            $query = $db->getQuery(true)
            ->select('b.*')
            ->from($db->quoteName('#__mywalk_dates') . ' AS b')
            ->where($db->quoteName('b.walk_id') . ' = :id')
            ->bind(':id', $pk, ParameterType::INTEGER)
            ->order($db->quoteName('date') . ' DESC');

            $db->setQuery($query);

            $data = $db->loadObjectList();

            // It is OK to have a walk without visit data - handle it the view.
        }
        catch (\Exception $e)
        {
            if ($e->getCode() == 404)
            {
                // Need to go through the error handler to allow Redirect to work.
                throw new \Exception($e->getMessage(), 404);
            }
            else
            {
                $this->setError($e);
                $this->_item[$pk] = false;
            }
        }

        return $data;
    }
}
```

#### Model file: MywalksModel

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\Model;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\Model\ListModel;
use Joomla\Database\ParameterType;

/**
 * This models supports retrieving lists of articles.
 *
 * @since  1.6
 */
class MywalksModel extends ListModel
{
    /**
     * Constructor.
     *
     * @param   array  $config  An optional associative array of configuration settings.
     *
     * @see     \JController
     * @since   1.6
     */
    public function __construct($config = array())
    {
        if (empty($config['filter_fields']))
        {
            $config['filter_fields'] = array(
                'id', 'a.id',
                'title', 'a.title',
                'distance', 'a.distance',
            );
        }

        parent::__construct($config);
    }

    /**
     * Method to auto-populate the model state.
     *
     * This method should only be called once per instantiation and is designed
     * to be called on the first call to the getState() method unless the model
     * configuration flag to ignore the request is set.
     *
     * Note. Calling getState in this method will result in recursion.
     *
     * @param   string  $ordering   An optional ordering field.
     * @param   string  $direction  An optional direction (asc|desc).
     *
     * @return  void
     *
     * @since   3.0.1
     */
    protected function populateState($ordering = 'a.id', $direction = 'ASC')
    {
        $search = $this->getUserStateFromRequest($this->context . '.filter.search', 'filter_search');
        $this->setState('filter.search', $search);

        // List state information.
        parent::populateState($ordering, $direction);
    }

    /**
     * Method to get a store id based on model configuration state.
     *
     * This is necessary because the model is used by the component and
     * different modules that might need different sets of data or different
     * ordering requirements.
     *
     * @param   string  $id  A prefix for the store id.
     *
     * @return  string  A store id.
     *
     * @since   1.6
     */
    protected function getStoreId($id = '')
    {
        // Compile the store id.
        $id .= ':' . $this->getState('filter.search');

        return parent::getStoreId($id);
    }

    /**
     * Get the master query for retrieving a list of walks subject to the model state.
     *
     * @return  \JDatabaseQuery
     *
     * @since   1.6
     */
    protected function getListQuery()
    {

        // Create a new query object.
        $db    = $this->getDatabase();
        $query = $db->getQuery(true);

        // Select the required fields from the table.
        $query->select(
            $this->getState(
                'list.select',
                'a.*,
                (SELECT MAX(' . $db->quoteName('date') 
                . ') FROM ' . $db->quoteName('#__mywalk_dates') 
                . ' WHERE ' . $db->quoteName('walk_id') . ' = ' . $db->quoteName('a.id') . ') AS last_visit,
                (SELECT count(' . $db->quote('date') . ') FROM ' . $db->quoteName('#__mywalk_dates') 
                . ' WHERE ' . $db->quoteName('walk_id') . ' = ' . $db->quoteName('a.id') . ') AS nvisits'
                )
        );
        $query->from($db->quoteName('#__mywalks') . ' AS a');

        // Filter by search in title.
        $search = $this->getState('filter.search');

        if (!empty($search))
        {
            $search = '%' . trim($search) . '%';
            $query->where($db->quoteName('a.title') . ' LIKE :search')
            ->bind(':search', $search, ParameterType::STRING);
        }

        // Add the list ordering clause.
        $orderCol  = $this->state->get('list.ordering', 'a.id');
        $orderDirn = $this->state->get('list.direction', 'ASC');

        if ($orderCol === 'title') {
            $ordering = [
                $db->quoteName('a.title') . ' ' . $db->escape($orderDirn),
            ];
        } else {
            $ordering = $db->escape($orderCol) . ' ' . $db->escape($orderDirn);
        }

        $query->order($ordering);

        return $query;
    }
}
```

## Control Flow

### Starting the component - the Controller

It is worth remembering that the non-SEF url for the list of walks page
is index.php?option=com_mywalks&task=display&view=mywalks. The *task*
part is often left out, in which case the default task is set to
*display*. If the *view* part is left out then the component must set
the default view.

Each page request starts with an initialisation sequence. That done, the
entry points to the component are via their controller files. The
default component view is *display* so it should be no surprise that the
default controller is DisplayController. This controller does not do
much other than call its parent controller. However, controllers can be
used to do initial processing. For example, if a form is being submitted
it is common practice to check the form token and *die* if it is
invalid.

#### DisplayController

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\Controller;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\Controller\BaseController;

/**
 * Mywalks Component Controller
 *
 * @since  1.5
 */
class DisplayController extends BaseController
{
    /**
     * Method to display a view.
     *
     * @param   boolean  $cachable   If true, the view output will be cached
     * @param   array    $urlparams  An array of safe URL parameters and their variable types, for valid values see {@link \JFilterInput::clean()}.
     *
     * @return  static  This object to support chaining.
     *
     * @since   1.5
     */
    public function display($cachable = false, $urlparams = array())
    {
        return parent::display();
    }
}
```

### Essential Administrator Files

Although we are still developing the code for the Site display, some
Administrator code is needed. The services/provider.php file is used to
boot the component, either to display its own site views or for use by
the menu module to create menu items.

#### The services provider file: administrator/components/com_mywalks/services/provider.php

Pay special notice to the lines starting
\$container-\>registerServiceProvider as this is where your code gets
registered in a container for use later.

``` php
<?php
/**
 * @package     Mywalks.Administrator
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

defined('_JEXEC') or die;

use Joomla\CMS\Component\Router\RouterFactoryInterface;
use Joomla\CMS\Dispatcher\ComponentDispatcherFactoryInterface;
use Joomla\CMS\Extension\ComponentInterface;
use Joomla\CMS\Extension\Service\Provider\CategoryFactory;
use Joomla\CMS\Extension\Service\Provider\ComponentDispatcherFactory;
use Joomla\CMS\Extension\Service\Provider\MVCFactory;
use Joomla\CMS\Extension\Service\Provider\RouterFactory;
use Joomla\CMS\HTML\Registry;
use Joomla\CMS\MVC\Factory\MVCFactoryInterface;
use J4xdemos\Component\Mywalks\Administrator\Extension\MywalksComponent;
use Joomla\DI\Container;
use Joomla\DI\ServiceProviderInterface;

/**
 * The mywalks service provider.
 *
 * @since  4.0.0
 */
return new class implements ServiceProviderInterface
{
    /**
     * Registers the service provider with a DI container.
     *
     * @param   Container  $container  The DI container.
     *
     * @return  void
     *
     * @since   4.0.0
     */
    public function register(Container $container)
    {
        $container->registerServiceProvider(new CategoryFactory('\\J4xdemos\\Component\\Mywalks'));
        $container->registerServiceProvider(new MVCFactory('\\J4xdemos\\Component\\Mywalks'));
        $container->registerServiceProvider(new ComponentDispatcherFactory('\\J4xdemos\\Component\\Mywalks'));
        $container->registerServiceProvider(new RouterFactory('\\J4xdemos\\Component\\Mywalks'));
        $container->set(
                ComponentInterface::class,
                function (Container $container)
                {
                    $component = new MywalksComponent($container->get(ComponentDispatcherFactoryInterface::class));

                    $component->setRegistry($container->get(Registry::class));
                    $component->setMVCFactory($container->get(MVCFactoryInterface::class));
                    $component->setRouterFactory($container->get(RouterFactoryInterface::class));

                    return $component;
        }
        );
    }
};
```

#### The component boot file: administrator/components/com_mywalks/src/Extension/MywalksComponent.php

``` php
<?php
/**
 * @package     Joomla.Administrator
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Administrator\Extension;

defined('JPATH_PLATFORM') or die;

use Joomla\CMS\Application\SiteApplication;
use Joomla\CMS\Component\Router\RouterServiceInterface;
use Joomla\CMS\Component\Router\RouterServiceTrait;
use Joomla\CMS\Extension\BootableExtensionInterface;
use Joomla\CMS\Extension\MVCComponent;
use Joomla\CMS\Factory;
use Joomla\CMS\HTML\HTMLRegistryAwareTrait;
use Psr\Container\ContainerInterface;

/**
 * Component class for com_mywalks
 *
 * @since  4.0.0
 */
class MywalksComponent extends MVCComponent implements
BootableExtensionInterface, RouterServiceInterface
{
    use RouterServiceTrait;
    use HTMLRegistryAwareTrait;

    /**
     * Booting the extension. This is the function to set up the environment of the extension like
     * registering new class loaders, etc.
     *
     * If required, some initial set up can be done from services of the container, eg.
     * registering HTML services.
     *
     * @param   ContainerInterface  $container  The container
     *
     * @return  void
     *
     * @since   4.0.0
     */
    public function boot(ContainerInterface $container)
    {
        //$this->getRegistry()->register('mywalksadministrator', new AdministratorService);
    }
}
```

For the moment the call to register the Administrator Service is
commented out. That results in a run-time error when invoking the
Mywalks component from the Administrator interface. Something to come
back to in Part 2.

### The Component Router

At this stage the com_mywalks component works. One menu item is needed
to link to the list of walks. There is snag: in the list of walks the
links to individual walks like like this:

`/site-root/my-walks.html?view=mywalk&id=1`

(where the site-root my or may not be a subfolder tree). Time for a
custom SEF router? And take a break to read [Supporting SEF URLs in your
component](https://docs.joomla.org/J3.x:Supporting_SEF_URLs_in_your_component).

For the mywalks component I want to use individual walk urls like this:

`/site-root/mywalks/walk-n/walk-title.html`

Where n is the individual walk id and the walk-title is automatically
generated from the actual title. Neither walk-title nor .html are
actually needed. The former is for friendliness and the latter because I
am old fashioned.

There are no menu items for individual walks. They are not wanted and
there is no way to generate them. A custom router is required,
consisting of two files: Router.php and MywalksNomenuRules.php. **These
are in the site section of the component code!**

#### The Router File: component/com_mywalks/src/Service/Router.php

``` php
<?php
/**
 * @package     Mywalks.Site
 * @subpackage  com_mywalks
 *
 * @copyright   Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\Service;

defined('_JEXEC') or die;

use Joomla\CMS\Application\SiteApplication;
use Joomla\CMS\Categories\CategoryFactoryInterface;
use Joomla\CMS\Component\ComponentHelper;
use Joomla\CMS\Component\Router\RouterView;
use Joomla\CMS\Component\Router\RouterViewConfiguration;
use Joomla\CMS\Component\Router\Rules\MenuRules;
use J4xdemos\Component\Mywalks\Site\Service\MywalksNomenuRules as NomenuRules;
use Joomla\CMS\Component\Router\Rules\StandardRules;
use Joomla\CMS\Menu\AbstractMenu;
use Joomla\Database\DatabaseInterface;

/**
 * Routing class of com_mywalks
 *
 * @since  3.3
 */
class Router extends RouterView
{
    protected $noIDs = false;

    /**
     * The category factory
     *
     * @var CategoryFactoryInterface
     *
     * @since  4.0.0
     */
    private $categoryFactory;

    /**
     * The db
     *
     * @var DatabaseInterface
     *
     * @since  4.0.0
     */
    private $db;

    /**
     * Mywalks Component router constructor
     *
     * @param   SiteApplication           $app              The application object
     * @param   AbstractMenu              $menu             The menu object to work with
     * @param   CategoryFactoryInterface  $categoryFactory  The category object
     * @param   DatabaseInterface         $db               The database object
     */
    public function __construct(SiteApplication $app, AbstractMenu $menu,
            CategoryFactoryInterface $categoryFactory, DatabaseInterface $db)
    {
        $this->categoryFactory = $categoryFactory;
        $this->db              = $db;

        $params = ComponentHelper::getParams('com_mywalks');
        $this->noIDs = (bool) $params->get('sef_ids');

        $mywalks = new RouterViewConfiguration('mywalks');
        $mywalks->setKey('id');
        $this->registerView($mywalks);

        $mywalk = new RouterViewConfiguration('mywalk');
        $mywalk->setKey('id');
        $this->registerView($mywalk);

        parent::__construct($app, $menu);

        $this->attachRule(new MenuRules($this));
        $this->attachRule(new StandardRules($this));
        $this->attachRule(new NomenuRules($this));
    }
}
```

Notice the lines that define and use custom rules:

``` php
    use Joomla\Component\Mywalks\Site\Service\MywalksNomenuRules as NomenuRules;
    ...
            $this->attachRule(new NomenuRules($this));
```

The rules include a *build* function to create the links to individual
walks, and a *parse* function to translate an incoming SEF url to an
internal Joomla route. There is no need to worry about the link in the
menu item as it is taken care of by the MenuRules rules.

#### The Router Rules file: components/my_walks/src/Service/MywalksNomenuRules.php

``` php
<?php
/**
 * Joomla! Content Management System
 *
 * @copyright  Copyright (C) 2005 - 2019 Open Source Matters, Inc. All rights reserved.
 * @license    GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Mywalks\Site\Service;

defined('JPATH_PLATFORM') or die;

use Joomla\CMS\Component\Router\RouterView;
use Joomla\CMS\Component\Router\Rules\RulesInterface;

/**
 * Rule to process URLs without a menu item
 *
 * @since  3.4
 */
class MywalksNomenuRules implements RulesInterface
{
    /**
     * Router this rule belongs to
     *
     * @var RouterView
     * @since 3.4
     */
    protected $router;

    /**
     * Class constructor.
     *
     * @param   RouterView  $router  Router this rule belongs to
     *
     * @since   3.4
     */
    public function __construct(RouterView $router)
    {
        $this->router = $router;
    }

    /**
     * Dummymethod to fullfill the interface requirements
     *
     * @param   array  &$query  The query array to process
     *
     * @return  void
     *
     * @since   3.4
     * @codeCoverageIgnore
     */
    public function preprocess(&$query)
    {
        $test = 'Test';
    }

    /**
     * Parse a menu-less URL
     *
     * @param   array  &$segments  The URL segments to parse
     * @param   array  &$vars      The vars that result from the segments
     *
     * @return  void
     *
     * @since   3.4
     */
    public function parse(&$segments, &$vars)
    {
        //with this url: http://localhost/j4x/my-walks/mywalk-n/walk-title.html
        // segments: [[0] => mywalk-n, [1] => walk-title]
        // vars: [[option] => com_mywalks, [view] => mywalks, [id] => 0]

        $vars['view'] = 'mywalk';
        $vars['id'] = substr($segments[0], strpos($segments[0], '-') + 1);
        array_shift($segments);
        array_shift($segments);
        return;
    }

    /**
     * Build a menu-less URL
     *
     * @param   array  &$query     The vars that should be converted
     * @param   array  &$segments  The URL segments to create
     *
     * @return  void
     *
     * @since   3.4
     */
    public function build(&$query, &$segments)
    {
        // content of $query ($segments is empty or [[0] => mywalk-3])
        // when called by the menu: [[option] => com_mywalks, [Itemid] => 126]
        // when called by the component: [[option] => com_mywalks, [view] => mywalk, [id] => 1, [Itemid] => 126]
        // when called from a module: [[option] => com_mywalks, [view] => mywalks, [format] => html, [Itemid] => 126]
        // when called from breadcrumbs: [[option] => com_mywalks, [view] => mywalks, [Itemid] => 126]

        // the url should look like this: /site-root/mywalks/walk-n/walk-title.html

        // if the view is not mywalk - the single walk view
        if (!isset($query['view']) || (isset($query['view']) && $query['view'] !== 'mywalk') || isset($query['format']))
        {
            return;
        }
        $segments[] = $query['view'] . '-' . $query['id'];
        // the last part of the url may be missing
        if (isset($query['slug'])) {
            $segments[] = $query['slug'];
            unset($query['slug']);
        }
        unset($query['view']);
        unset($query['id']);
    }
}
```

When there is a menu item for a mywalks list page the MywalksNomenuRules
build function will be called for every internal link in a page: in
modules, menus and even content articles. So watch out for run-time
error messages.

## And Finally

That is it - a working component, but only working on the site side!

<img alt="The Mywalks site walks list" src="https://docs.joomla.org//images/2/2a/Mywalks-site-walks-list.jpg" decoding="async" data-file-width="1000" data-file-height="423" width="1000" height="423">
