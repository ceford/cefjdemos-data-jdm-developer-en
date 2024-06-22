<!-- Filename: J4.x:MVC_Anatomy:_Administrator_Edit_Files / Display title: MVC Anatomy: Administrator Edit Files -->

## Country Data Files

The administrator files include four views, two list views for the
countries data and currencies data and two edit views for managing data
entry in each of the two tables involved. The list views are almost
identical to the countries list view described for the site interface so
will not be covered again here. Similarly, the edit views are almost
identical so only one will be covered, the data entry view for a single
country.

## src/Controller/CountryController.php

This is immensely simple! Apart from the class declaration it is devoid
of content. Everything is taken care of by the parent class.

```php
    class CountryController extends FormController
    {
    }
```

## src/View/Country/HtmlView.php

This is where data is fetched from the model for use in the data entry
form. There is a significant difference from the list view described
previously: it is common practice to use a toolbar with buttons for
Save, Cancel, Help, etcetera.

When you invoke the edit form by selecting a list item title the url
contains an id, for example
option=com_countrybase&view=country&layout=edit&id=1. The id is the
table record number. For a new record invoked via the list New button
the id is absent. If it is present, the model fills out the data entry
form with the existing record data.

```php
       public function display($tpl = null)
        {
            $this->form  = $this->get('Form');
            $this->item  = $this->get('Item');
            $this->state = $this->get('State');

            if (count($errors = $this->get('Errors')))
            {
                throw new GenericDataException(implode("\n", $errors), 500);
            }

            $this->addToolbar();

            return parent::display($tpl);
        }
```

### The Toolbar

The addToolbar() function is typically used to set the page title and
add appropriate buttons for the access permissions of the person using
the form. Notice the use of the \$isNew variable based on the record id
to slightly modify the output.

```php
       protected function addToolbar()
        {
            Factory::getApplication()->input->set('hidemainmenu', true);
            $isNew      = ($this->item->id == 0);

            $canDo = ContentHelper::getActions('com_countrybase');

            $toolbar = Toolbar::getInstance();

            ToolbarHelper::title(
                Text::_('COM_COUNTRYBASE_COUNTRY_PAGE_TITLE_' . ($isNew ? 'ADD' : 'EDIT'))
            );

            if ($canDo->get('core.create'))
            {
                if ($isNew)
                {
                    $toolbar->apply('country.save');
                }
                else
                {
                    $toolbar->apply('country.apply');
                }
                $toolbar->save('country.save');

            }
            $toolbar->cancel('country.cancel', 'JTOOLBAR_CLOSE');

            ToolbarHelper::inlinehelp();
        }
```

**Notes:**

- **hidemainmenu** is set to true for all edit forms to prevent you
  leaving the form via the Administrator menu without saving.
- **ToolbarHelper::title()** sets the title in the page title bar, in
  this case to Edit Country or Add Country.
- **toolbar-\>action** buttons, where action can be save, apply, cancel
  or one of many others, take (controller.function) as arguments. So in
  this case, when a button is selected action is passed to a save or an
  apply function in the CountryController class.
- **cancel** takes an additional argument, the string key used to label
  the button.
- **inlinehelp()** shows a button that toggles on or off field
  descriptions below each data entry field.

## forms/country.xml

This file contains the specification of the form fields, usually in the
order presented in the edit form. Most of the fields are
self-explanatory so only two are shown below. The name values are the
column names used in the database tables.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<form>

		<field
			name="title"
			type="text"
			label="COM_COUNTRYBASE_COUNTRY_TITLE"
			required="true"
			maxlength="128"
		/>

		<field
			name="iso_2"
			type="text"
			label="COM_COUNTRYBASE_COUNTRY_ISO_2"
			description="COM_COUNTRYBASE_COUNTRY_ISO_2_DESC"
			required="true"
			maxlength="3"
		/>
```

## src/Model/CountryModel.php

The functions in this file are fairly standard. However, the getTable()
function requires some explanation.

```php
       public function getTable($name = '', $prefix = '', $options = array())
        {
            $name = 'Countries';
            $prefix = 'Table';

            if ($table = $this->_createTable($name, $prefix, $options))
            {
                return $table;
            }

            throw new \Exception(Text::sprintf('JLIB_APPLICATION_ERROR_TABLE_NAME_NOT_SUPPORTED', $name), 0);
        }
```

This function does not refer to the table itself! It refers to the
constructor of src/Table/CountriesTable.php. This can be a little
confusing because it is being called from the CountryModel class.

## src/Table/CountriesTable.php

So this is where the table name for the countries list is defined.

```php
    class CountriesTable extends Table
    {
        /**
         * Constructor
         *
         * @param   DatabaseDriver  $db  Database connector object
         *
         * @since   1.6
         */
        public function __construct(DatabaseDriver $db)
        {
            parent::__construct('#__countrybase_countries', 'id', $db);

            $this->setColumnAlias('published', 'state');
        }
    }
```

Notice declaration of the column alias. This is used because the form
uses a data entry field named **published** but the field used for
storage is named **status**.

## tmpl/country/edit.php

This is the file that creates the HTML output. It starts with two
helpers: HTMLHelper::\_('behavior.formvalidator'); loads the javascript
needed to for client side form validation. Although modern browsers
apply form validation that does not prevent form submission.
HTMLHelper::\_('behavior.keepalive'); loads javscript to ping the server
to prevent session expiry whilst completing complex forms.

**Notes:**

- LayoutHelper::render('joomla.edit.title_alias', \$this); renders a
  standard Joomla title field.
- \$this-\>form-\>renderField('iso_2'); renders the specified field, in
  this case iso_2.
- LayoutHelper::render('joomla.edit.global', \$this); renders the fields
  on the right side of the Details tab.

```php
<?php
/**
 * @package     Countrybase.Administrator
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

HTMLHelper::_('behavior.formvalidator');
HTMLHelper::_('behavior.keepalive');

?>

<form action="<?php echo Route::_('index.php?option=com_countrybase&view=country&layout=edit&id=' . (int) $this->item->id); ?>"
	method="post" name="adminForm" id="country-form" class="form-validate">

	<?php echo LayoutHelper::render('joomla.edit.title_alias', $this); ?>

	<div>
		<?php echo HTMLHelper::_('uitab.startTabSet', 'myTab', array('active' => 'details')); ?>

		<?php echo HTMLHelper::_('uitab.addTab', 'myTab', 'details', Text::_('COM_COUNTRYBASE_COUNTRY_TAB_DETAILS')); ?>
		<div class="row">
			<div class="col-md-9">
				<div class="row">
					<div class="col-md-6">
						<?php echo $this->form->renderField('iso_2'); ?>
						<?php echo $this->form->renderField('iso_3'); ?>
						<?php echo $this->form->renderField('country_code'); ?>
						<?php echo $this->form->renderField('region_code'); ?>
						<?php echo $this->form->renderField('subregion_code'); ?>
						<?php echo $this->form->renderField('phone_prefix'); ?>
						<?php echo $this->form->renderField('currency_code'); ?>
						<?php echo $this->form->renderField('id'); ?>
					</div>
				</div>
			</div>
			<div class="col-md-3">
				<div class="card card-light">
					<div class="card-body">
						<?php echo LayoutHelper::render('joomla.edit.global', $this); ?>
					</div>
				</div>
			</div>
		</div>
		<?php echo HTMLHelper::_('uitab.endTab'); ?>

		<?php echo HTMLHelper::_('uitab.endTabSet'); ?>
	</div>
	<input type="hidden" name="task" value="">
	<?php echo HTMLHelper::_('form.token'); ?>
</form>
```

You can cycle through form fieldsets and fields within each fieldset.
That can simplify output of complex forms with many tabs.

<img
src="https://docs.joomla.org/images/thumb/7/79/J4.x-mvc-anatomy-country-edit-form-en.png/800px-J4.x-mvc-anatomy-country-edit-form-en.png"
class="thumbborder" decoding="async"
srcset="https://docs.joomla.org/images/thumb/7/79/J4.x-mvc-anatomy-country-edit-form-en.png/1200px-J4.x-mvc-anatomy-country-edit-form-en.png 1.5x, https://docs.joomla.org/images/7/79/J4.x-mvc-anatomy-country-edit-form-en.png 2x"
data-file-width="1440" data-file-height="1162" width="800" height="646"
alt="J4.x-mvc-anatomy-country-edit-form-en.png" />

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
