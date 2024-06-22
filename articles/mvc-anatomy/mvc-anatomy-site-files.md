<!-- Filename: J4.x:MVC_Anatomy:_Site_Files / Display title: MVC Anatomy: Site Files -->

## File Structure

There are fewer files in the Site part of the component than in the
Administrator part so this seems like a good place to start. Only those
parts of each file that need explanation will be covered. It is best if
you open each file being mentioned, have a look at the overall content
and then find the parts being explained. The alphabet order of the file
structure is as follows:

```bash
    Site
     |- forms
        |- filter_countries.xml
     |- language
        |- en-GB
           |- com_countrybase.ini
     |- src
        |- Controller
           |- DisplayController
        |- Model
           |- CountriesModel.php
        |- Service
           |- Router.php
        |- View
           |- Countries
              |- HtmlView.php
     |- tmpl
        |- countries
           |- default.php
           |- default.xml
```

## DisplayContoller.php

The complete file in a scrollable div:

```php
<?php
/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2022 Clifford E Ford
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

namespace J4xdemos\Component\Countrybase\Site\Controller;

defined('_JEXEC') or die;

use Joomla\CMS\MVC\Controller\BaseController;
use Joomla\CMS\Router\Route;
use Joomla\CMS\Session\Session;

/**
 * Countrybase Component Controller
 *
 * @since  4.0.0
 */
class DisplayController extends BaseController
{
	/**
	 * The default view.
	 *
	 * @var    string
	 * @since  1.6
	 */
	protected $default_view = 'countries';

	protected $app;
}
```

Parts of this file are explained as follows:

### Copyright Notice

Every php file should start with a copyright notice like the following:

```php
<?php
/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2022 Clifford E Ford
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */
```

If you frequently create new files and copy/paste this section remember to update the component name and copyright notice.

### Namespace and defined check

Following the copyright notice every php file must have a line containing defined('_JEXEC') or die; except that namespaced files must declare the namespace before any other php code, so before the defined check. Namespaced php files are those containing component php classes in the src folder or its sub-folders.

```php
    namespace J4xdemos\Component\Countrybase\Site\Controller;

    defined('_JEXEC') or die;
```

The defined check prevents a php file from being executed by calling it
directly via its url. The \_JEXEC constant is defined when the
application starts via the root or administrator index.php file. It is
an important security aid.

Combined with the namespace declared in in the countrybase.xml manifest
file, Joomla will look for any class declared in the current file in
root/components/com_countrybase/src/Controller - in this case appending
the name of this file, DisplayController.php

### use Statements

The use statements usually follow the defined check and are often listed
in alphabet order. The use statements define the locations of the
classes used by this php file. Sometimes, use statements are there by
mistake, being declared but not used. That does no harm but ought to be
corrected. There are two unused statements here:

```php
    use Joomla\CMS\MVC\Controller\BaseController;
    use Joomla\CMS\Router\Route;
    use Joomla\CMS\Session\Session;
```

### Controller Class

The display controller has almost nothing to do as all the work is done
in the parent class. The one important thing it does is set the default
view, in this case **countries**. That will cause the default component
view to use the Countries/HtmlView.php and tmpl/countries/default.php
files to display the countries data.

```php
    /**
     * Countrybase Component Controller
     *
     * @since  4.0.0
     */
    class DisplayController extends BaseController
    {
        /**
         * The default view.
         *
         * @var    string
         * @since  1.6
         */
        protected $default_view = 'countries';

        protected $app;
    }
```

The code layout is a Joomla standard layout for php (<a
href="https://developer.joomla.org/coding-standards/basic-guidelines.html"
class="external free" target="_blank"
rel="noreferrer noopener">https://developer.joomla.org/coding-standards/basic-guidelines.html</a>).
The documentation blocks are designed for automated documentation of
code. The **since** values illustrated here are for Joomla 4 code.

Do not forget to set the \$default_view for this controller. Without it,
the DisplayController will use the the default view defined in the
component configuration file or, if that does not exist, the component
name.

## src/View/Countries/HtmlView.php

The controller has set the default view to **countries** so the next
step is to load the corresponding HtmlView.php code. Parts of this file
requiring some explanation.

### Class variables

```php
    class HtmlView extends BaseHtmlView
    {
        /**
         * The model state
         *
         * @var  \Joomla\CMS\Object\CMSObject
         */
        protected $state = null;
        ...
        protected $items = null;
        ...
        protected $pagination = null;
        ...
        public $filterForm;
        ...
        public $activeFilters;
```

The class variables are used to store information about the page to be
displayed:

- \$state - the model state, often primed by form or query string input.
- \$items - the list of country data retrieved from the database.
- \$pagination - an object used to display a page navigation mechanism
  if there are more pages than the list limit, normally 20 items.
- \$filterForm - not usually found in site pages but used in
  com_countrybase to filter by country name or published state.
- \$activeFilters - used to keep track of which filters are in use.

### display function

This is fairly standard for a site view, apart from the filterForm and
activeFilters parts:

```php
       public function display($tpl = null)
        {
            $this->state      = $this->get('State');
            $this->items      = $this->get('Items');
            $this->pagination = $this->get('Pagination');
            $this->filterForm    = $this->get('FilterForm');
            $this->activeFilters = $this->get('ActiveFilters');

            // Flag indicates to not add limitstart=0 to URL
            $this->pagination->hideEmptyLimitstart = true;

            // Check for errors.
            if (count($errors = $this->get('Errors')))
            {
                throw new GenericDataException(implode("\n", $errors), 500);
            }

            parent::display($tpl);
        }
```

The statements of the form \$this-\>get('Xxxx') cause Joomla to look in
CountriesModel.php for a function named getXxxx() and return any data
executed by that code for storage and use in the view. Often the
function is in the CountriesModel parent. For example the getItems
function is not present in CountriesModel.php but is present in the
ListModel that it extends.

In summary the view class retrieves data from the model and then calls
its parent class to display the data.

## Model/CountriesModel.php

There are a small number of functions in the Model that you usually need
to complete yourself. Others are inherited from the parent ListModel.

### constructor

It is normal practice to include in the constructor any filter fields
that you may wish to use. Without them, filters may appear to have no
effect. They are often forgotten when you wish to add a filter some time
later. Each field is given as its field name and with a table name
alias, usually a, b, c and so on, but can be anything you choose that is
consistent with code elsewhere.

```php
       public function __construct($config = array())
        {
            if (empty($config['filter_fields']))
            {
                $config['filter_fields'] = array(
                        'id', 'a.id',
                        'title', 'a.title',
                        'iso_2', 'a.iso_2',
                        'iso_3', 'a.iso_3',
                        'country_code', 'a.country_code',
                        'region_code', 'a.region_code',
                        'state', 'a.state',
                        'subregion_code', 'a.subregion_code',
                        'phone_prefix', 'a.phone_prefix',
                        'currency_code', 'a.currency_code',
                );
            }

            parent::__construct($config);
        }
```

### populateState

This functions takes input parameters and prepares them for use in a
database query. Some parameters are handled in the parent so there is no
need to do anything here. For example, if the search field is **title**
that is handled by the parent, as are the **state** and pagination
**start** and **limit** fields.

```php
       protected function populateState($ordering = 'title', $direction = 'ASC')
        {
            // List state information.
            parent::populateState($ordering, $direction);
        }
```

### getStoreId

This function creates a hash to store a query for use elsewhere.

```php
       protected function getStoreId($id = '')
        {
            // Compile the store id.
            $id .= ':' . $this->getState('filter.search');
            $id .= ':' . $this->getState('filter.published');

            return parent::getStoreId($id);
        }
```

### getListQuery

This is where the query is created to extract data from the database. it
requires some understanding of how queries are constructed.

```php
       protected function getListQuery()
        {
            // Create a new query object.
            $db    = $this->getDbo();
            $query = $db->getQuery(true);

            // Select the required fields from the table.
            $query->select(
                $this->getState(
                    'list.select',
                    [
                        $db->quoteName('a.title'),
                        $db->quoteName('a.iso_2'),
                        $db->quoteName('a.iso_3'),
                        $db->quoteName('a.country_code'),
                        $db->quoteName('a.region_code'),
                        $db->quoteName('a.subregion_code'),
                        $db->quoteName('a.phone_prefix'),
                        $db->quoteName('a.currency_code'),
                        $db->quoteName('a.state'),
                        $db->quoteName('b.title') . ' AS currency_title',
                        $db->quoteName('b.symbol'),
                        $db->quoteName('b.dollar_exchange_rate'),
                    ]
                )
            )
            ->from($db->quoteName('#__countrybase_countries', 'a'))
            ->leftjoin($db->quoteName('#__countrybase_currencies', 'b') . 'ON a.currency_code = b.currency_code');

            // Filter by search in title.
            $search = $this->getState('filter.search');

            if (!empty($search))
            {
                $search = $db->quote('%' . str_replace(' ', '%', $db->escape(trim($search), true) . '%'));
                $query->where('(a.title LIKE ' . $search . ')');
            }

            // Filter by published state
            $published = (string) $this->getState('filter.published');

            if ($published !== '*')
            {
                if (is_numeric($published))
                {
                    $state = (int) $published;
                    $query->where($db->quoteName('a.state') . ' = :state')
                    ->bind(':state', $state, ParameterType::INTEGER);
                }
            }

            // Add the list ordering clause.
            $orderCol  = $this->state->get('list.ordering', 'a.title');
            $orderDirn = $this->state->get('list.direction', 'ASC');

            $query->order($db->escape($orderCol) . ' ' . $db->escape($orderDirn));
            return $query;
        }
```

Explanation

- **getQuery(true)** gets a new empty query object.
- **\$query-\>select()** adds a SELECT statement. There can be multiple
  select statements - Joomla concatenates them.
- **table aliases a and b** notice that some columns come from different
  tables.
- **-\>from()** defines which table is table a. this can be in a
  separate statement: \$query-\>from();
- **-\>leftjoin()** defines table b and how it is to be joined to table
  a.
- **\$query-\>where()** makes use of any filters defined, one for
  **search** and another for **state**.
- **return \$query** there is no parent call, everything in the query
  must be set up here.

## tmpl/countries/default.php

This is the part of the code where the html content is created. At the
very least it may just contain `<h1>Hello World</h1>`. For the list of 
countries a table is needed with a heading row and one
row for data on each country. As there are 250 countries a pagination
mechanism is needed to display a subset of countries a few at a time.
That requires a form. And in this case a standard Joomla searchtools bar
is useful. This is it:

```php
<?php
/**
 * @package     Countrybase.Site
 * @subpackage  com_countrybase
 *
 * @copyright   (C) 2022 Clifford E Ford
 * @license     GNU General Public License version 2 or later; see LICENSE.txt
 */

\defined('_JEXEC') or die;

use Joomla\CMS\HTML\HTMLHelper;
use Joomla\CMS\Language\Text;
use Joomla\CMS\Layout\LayoutHelper;
use Joomla\CMS\Router\Route;

$listOrder = $this->escape($this->state->get('list.ordering'));
$listDirn  = $this->escape($this->state->get('list.direction'));

?>
<h1><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES'); ?></h1>

<form action="<?php echo Route::_('index.php?option=com_countrybase'); ?>" method="post" name="adminForm" id="adminForm">

<?php echo LayoutHelper::render('joomla.searchtools.default', array('view' => $this)); ?>

<div class="table-responsive">
	<table class="table table-striped">
	<caption><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION'); ?></caption>
	<thead>
	<tr>
		<th scope="col">
			<?php echo HTMLHelper::_('searchtools.sort', 'COM_COUNTRYBASE_COUNTRIES_COUNTRY', 'a.title', $listDirn, $listOrder); ?>
		</th>
		<th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_2'); ?></th>
		<th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_ISO_3'); ?></th>
		<th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE'); ?></th>
		<th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL'); ?></th>
		<th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE'); ?></th>
		<th scope="col"><?php echo Text::_('COM_COUNTRYBASE_COUNTRIES_XRATE'); ?></th>
	</tr>
	</thead>
	<tbody>
	<?php foreach ($this->items as $id => $item) : ?>
	<tr>
		<td><?php echo $item->title; ?></td>
		<td><?php echo $item->iso_2; ?></td>
		<td><?php echo $item->iso_3; ?></td>
		<td><?php echo $item->currency_title; ?></td>
		<td><?php echo $item->symbol; ?></td>
		<td><?php echo $item->currency_code; ?></td>
		<td><?php echo $item->dollar_exchange_rate; ?></td>
	</tr>
	<?php endforeach; ?>
	</tbody>
	</table>
</div>

<?php echo $this->pagination->getListFooter(); ?>

<input type="hidden" name="task" value="">
<input type="hidden" name="boxchecked" value="0">
<?php echo HTMLHelper::_('form.token'); ?>

</form>
```

Points to note:

- **\$listOrder** and **\$listDirection** are used to order by column
  heading. Only the title has been set up to do that.
- **form action** is typically set up to refer to itself.
- **LayoutHelper::render('joomla.searchtools.default',...)** creates a
  search bar of the type seen in administrator list pages. **It needs a
  filter form!**
- **\$this-\>pagination-\>getListFooter()** fetches the html code for
  the pagination widget.
- **task** this hidden field is filled out by javascript when the form
  is submitted.
- **boxchecked** this hidden field is used when one or more row
  checkboxes are selected for batch operation. Not really needed here!
- **HTMLHelper::\_('form.token');** gets the code for a form token used
  as a security device for form submissions involving data submission.
  Not really needed here!

## tmpl/countries/default.xml

This file is used to create a menu item. It has the same name as the php
file, so default.xml in this case.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<metadata>
	<layout title="COM_COUNTRYBASE_VIEW_DEFAULT_MENU_LABEL" 
		option="COM_COUNTRYBASE_VIEW_DEFAULT_OPTION">
		<help
			url="components/com_countrybase/help/en-GB/countrybase.html"
		/>
		<message>
			<![CDATA[COM_COUNTRYBASE_VIEW_DEFAULT_MENU_DESC]]>
		</message>
	</layout>

	<!-- Add fields to the parameters object for the layout. -->
	<fields name="params">

		<!-- Options -->
		<fieldset name="options">
		</fieldset>

	</fields>
</metadata>
```

Notes:

- **help url** points to a help file in the administrator folder. It
  allows you to create your own local help files, invoked from the menu
  edit form Help button after selecting the Countrybase Default View
  menu type.
- **params** allows you to use parameters, for example whether or not to
  show a particular column in the countries list. No parameters have
  been specified yet.
- **key** translations need to be in the
  administrator/language/en-GB/countrybase.sys.ini file.

## forms/filter_countries.xml

This file is needed for the search bar. Without it, Joomla will throw a
fatal error. The file name must be exactly as shown: the view name
preceded by filter\_. The content is simple, just definitions for the
search field and any other filters you may wish to use.

```xml
<?xml version="1.0" encoding="utf-8"?>
<form>

	<fields name="filter">

		<field
			name="search"
			type="text"
			label="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL"
			description="COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC"
			hint="JSEARCH_FILTER"
		/>

		<field
			name="published"
			type="status"
			label="JOPTION_SELECT_PUBLISHED"
			onchange="this.form.submit();"
			>
			<option value="">JOPTION_SELECT_PUBLISHED</option>
		</field>

	</fields>
```

Note that any string keys starting with J are Joomla defined and you
should not include them in your language files.

## language/en-GB/com_countrybase.ini

Joomla always loads English language keys before any other language.
This ensures that keys do not appear in the output if a language is
incompletely translated. Because the language keys are long words in
pseudo-English it is considered better to have a mixture of English and
another language than a mixture of keys and another language. If another
language is in use Joomla overwrites the English strings with those of
the other language.

Note that it is common practice to list the strings in key alphabet
order:

```ini
    ; Joomla! Project
    ; (C) 2005 Open Source Matters, Inc. https://www.joomla.org
    ; License GNU General Public License version 2 or later; see LICENSE.txt
    ; Note : All ini files need to be saved as UTF-8

    COM_COUNTRYBASE_COUNTRIES_COUNTRY="Country"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_CODE="Code"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_SYMBOL="Symbol"
    COM_COUNTRYBASE_COUNTRIES_CURRENCY_TITLE="Currency"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_ASC="Country ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_COUNTRY_DESC="Country DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_ASC="Currency code ASC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_CURRENCY_CODE_DESC="Currency code DESC"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_DESC="Search in Country Name"
    COM_COUNTRYBASE_COUNTRIES_FILTER_SEARCH_LABEL="Search"
    COM_COUNTRYBASE_COUNTRIES_ISO_2="ISO2"
    COM_COUNTRYBASE_COUNTRIES_ISO_3="ISO3"
    COM_COUNTRYBASE_COUNTRIES_TABLE_CAPTION="Table of Country Currencies"
    COM_COUNTRYBASE_COUNTRIES_XRATE="Exchange Rate"
    COM_COUNTRYBASE_COUNTRIES="Countries"
```

## src/Service/Router.php

The Router is required for SEO urls. Without it a menu link may appear
as option=com_countrybase&view=countries. With it a link will appear as
country-base.html or whatever name you choose for the link title alias.

```php
       public function __construct(SiteApplication $app, AbstractMenu $menu,
                CategoryFactoryInterface $categoryFactory, DatabaseInterface $db)
        {

            $countries = new RouterViewConfiguration('countries');
            $countries->setKey('id');
            $this->registerView($countries);

            parent::__construct($app, $menu);

            $this->attachRule(new MenuRules($this));
            $this->attachRule(new StandardRules($this));
            $this->attachRule(new NomenuRules($this));
        }
```

If there are more views, for example a table of currencies, you would
define each view here before the parent::\_\_construct() statement.

## Related Tutorials

### MVC Anatomy

- [Getting
  Started](https://docs.joomla.org/J4.x:MVC_Anatomy:_Getting_Started "Special:MyLanguage/J4.x:MVC Anatomy: Getting Started")
- [File
  Structure](https://docs.joomla.org/J4.x:MVC_Anatomy:_File_Structure "Special:MyLanguage/J4.x:MVC Anatomy: File Structure")
- [Manifest
  File](https://docs.joomla.org/J4.x:MVC_Anatomy:_Manifest_File "Special:MyLanguage/J4.x:MVC Anatomy: Manifest File")
- [Site
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Site_Files "Special:MyLanguage/J4.x:MVC Anatomy: Site Files")
- [Administrator Startup
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Administrator_Startup_Files "Special:MyLanguage/J4.x:MVC Anatomy: Administrator Startup Files")
- [Administrator Edit
  Files](https://docs.joomla.org/J4.x:MVC_Anatomy:_Administrator_Edit_Files "Special:MyLanguage/J4.x:MVC Anatomy: Administrator Edit Files")
