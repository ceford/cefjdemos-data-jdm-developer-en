<!-- Filename: J4.x:Creating_a_Plugin_for_Joomla / Display title: Creating a Plugin for Joomla -->

<span id="main-portal-heading">Tutorial  
How to create a Plugin for Joomla 4</span> Joomla!  4.x series

The plugin structure for Joomla! 1.5, 2.5 and 3.x was very flexible and
powerful. Not only can plugins be used to handle events triggered by the
core application and extensions, but plugins can also be used to make
third party extensions extensible and powerful. In Joomla 4.x we have
rewritten a lot of the dispatcher system behind this to increase the
flexibility further when you modify the parameters passed as events
whilst simultaneously increasing the performance of plugins.

This How-To should provide you with the basics of what you need to know
to develop your own plugin. Most plugins consist of just a single code
file but to correctly install the plugin code it must be packaged into
an installation file which can be processed by the Joomla installer.

### Creating the Installation File

As with all extensions in Joomla, plugins are easily installed as a .zip
file (.tar.gz is also supported) but a correctly formatted XML file must
be included.  
As an example, here is the XML installation file for the categories
search plugin:

```xml
<?xml version="1.0" encoding="utf-8"?>
<extension type="plugin" group="search" method="upgrade">
	<name>plg_search_categories</name>
	<author>Joomla! Project</author>
	<creationDate>November 2005</creationDate>
	<copyright>Copyright (C) 2005 - 2018 Open Source Matters. All rights reserved.</copyright>
	<license>GNU General Public License version 2 or later; see LICENSE.txt</license>
	<authorEmail>admin@joomla.org</authorEmail>
	<authorUrl>www.joomla.org</authorUrl>
	<version>3.0.0</version>
	<description>PLG_SEARCH_CATEGORIES_XML_DESCRIPTION</description>
	<files>
		<filename plugin="categories">categories.php</filename>
	</files>
	<languages>
		<language tag="en-GB">en-GB.plg_search_categories.ini</language>
		<language tag="en-GB">en-GB.plg_search_categories.sys.ini</language>
	</languages>
	<config>
		<fields name="params">

			<fieldset name="basic">
				<field
					name="search_limit"
					type="number"
					label="JFIELD_PLG_SEARCH_SEARCHLIMIT_LABEL"
					default="50"
				/>

				<field
					name="search_content"
					type="radio"
					label="JFIELD_PLG_SEARCH_ALL_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>

				<field
					name="search_archived"
					type="radio"
					label="JFIELD_PLG_SEARCH_ARCHIVED_LABEL"
					layout="joomla.form.field.radio.switcher"
					default="0"
					>
					<option value="1">JYES</option>
					<option value="0">JNO</option>
				</field>
			</fieldset>

		</fields>
	</config>
</extension>
```

As you can see, the system is similar to other Joomla XML installation
files. You only have to look out for the `group="xxx"` entry in the tag
and the extended information in the tag. This information tells Joomla
into which folder to copy the file and to which group the plugin should
be added.

If you are creating a plugin that responds to existing core events, the
`group="xxx"` attribute would be changed to reflect the name of existing
plugin folder for the event type you wish to augment. e.g.
`group="authentication"` or `group="user"`. See
[Plugin/Events](https://docs.joomla.org/Plugin/Events "Special:MyLanguage/Plugin/Events")
for a complete list of existing core event categories. In creating a new
plugin to respond to core events it is important that your plugin's name
is unique and does not conflict with any of the other plugins that may
also be responding to the core event you wish to service as well.

If you are creating a plugin to respond to non-core system events your
choice for the `group="xxx"` tag should be different than any of the
existing core categories.

**TIP:** *If you add the attribute `method="upgrade"` to the tag
`extension`, this plugin can be installed without uninstalling an
earlier version. All existing files will be overwritten, but old files
will not be deleted.*

### Creating the Plugin

In Joomla 4 you can also have namespaced code in your plugin - please
read [Namespace Conventions in Joomla
4](https://docs.joomla.org/J4.x:Namespace_Conventions_In_Joomla "J4.x:Namespace Conventions In Joomla").
Note the main entry file is not namespaced but fields and any other
supporting code is.

The object-oriented way of writing plugins involves writing a subclass
of <a
href="https://api.joomla.org/cms-4/classes/Joomla.CMS.Plugin.CMSPlugin.html"
class="external text" target="_blank"
rel="noreferrer noopener">CMSPlugin</a>, a base class that implements
the basic properties of plugins. In your methods, the following
properties are available:

- `$this->params`: the
  [parameters](https://docs.joomla.org/Parameter "Special:MyLanguage/Parameter")
  set for this plugin by the administrator
- `$this->_name`: the name of the plugin
- `$this->_type`: the group (type) of the plugin
- `$this->db`: the db object
- `$this->app`: the application object

**TIP:** *To use `$this->db` and `$this->app`, `CMSPlugin` tests if the
property exists and is not private. If it is desired for the default
objects to be used, create un-instantiated properties in the plugin
class (i.e. `protected $db; protected $app;` in the same area as
`protected $autoloadLanguage = true;`). The properties will not exist
unless explicitly created.*

In the following code example, represents the group (type) of the
plugin, and represents its name. Note that class and function names in
PHP are case-insensitive.

We also implement the <a
href="https://api.joomla.org/framework-2/classes/Joomla.Event.SubscriberInterface.html"
class="external text" target="_blank"
rel="noreferrer noopener">SubscriberInterface</a> here which is the
major change from Joomla 1.5-3.x. Instead of the function name
automatically being detected and being the same as the event name this
allows you to have custom function names. This allows us to tell what
plugins are implementing what functions and as parsing public methods in
PHP code is slow gives a significant performance boost.

Note throughout the Joomla 4 series there is a deprecated layer that
will cover plugins using the old naming strategy of plugin names being
the same as the event name when SubscriberInterface is not implemented.

```php
<?php
// no direct access
defined( '_JEXEC' ) or die;

use Joomla\CMS\Plugin\CMSPlugin;
use Joomla\Event\Event;
use Joomla\Event\SubscriberInterface;

class Plg<PluginGroup><PluginName> extends CMSPlugin implements SubscriberInterface
{
	/**
	 * Load the language file on instantiation
	 *
	 * @var    boolean
	 * @since  3.1
	 */
	protected $autoloadLanguage = true;

	/**
	 * Returns an array of events this subscriber will listen to.
	 *
	 * @return  array
	 */
	public static function getSubscribedEvents(): array
	{
		return [
			'<EventName>' => 'myFunctionName',
		];
	}

	/**
	 * Plugin method is the array value in the getSubscribedEvents method
	 * The plugin then modifies the Event object (if it's not immutable)
	 */
	 public function myFunctionName(Event $event)
	 {
		/*
		 * Plugin code goes here.
		 * You can access parameters via $this->params
		 */
		return true;
	}
}
?>
```

### Using Plugins in Your Code

If you are creating a plugin for a new, non-core event, remember to
activate your plugin after you install it. Precede any reference to your
new plugin with the `JPluginHelper::importPlugin()` command.

Now that you've created your plugin, you will probably want to call it
in your code. You might not: the Joomla core has a number of built-in
events that you might want your plugin code to be registered to (and in
that case you can ignore this section).

#### New Joomla 4 Way

The new way of doing this in Joomla 4 is to get the dispatcher and
dispatch a named event.

```php
    use Joomla\CMS\Event\AbstractEvent;
    use Joomla\CMS\Factory;

    $dispatcher = Factory::getApplication()->getDispatcher();

    // Here we create an event however as long as you implement EventInterface you can create your own 
    // custom classes
    $event = AbstractEvent::create(
        '',
        [
            'name' => $value,
        ]
    );

    $eventResult = $dispatcher->dispatch('', $event);
```

If you want to allow the user to modify values you can then use the
event result and getResults back out of it. You can look at

```php
    defined('_JEXEC') or die;

    use BadMethodCallException;
    use Joomla\CMS\Event\AbstractImmutableEvent;
    use Joomla\CMS\Table\TableInterface;

    /**
     * Event class for an event
     */
    class MyCustomEvent extends AbstractImmutableEvent
    {
        /**
         * Constructor.
         *
         * @param   string  $name       The event name.
         * @param   array   $arguments  The event arguments.
         *
         * @throws  BadMethodCallException
         */
        public function __construct($name, array $arguments = array())
        {
            if (!array_key_exists('myProperty', $arguments))
            {
                throw new BadMethodCallException("Argument 'myProperty' is required for event $name");
            }

            parent::__construct($name, $arguments);
        }

        /**
         * Setter for the myProperty argument
         *
         * @param   mixed  $value  The value to set
         *
         * @return  mixed
         *
         * @throws  BadMethodCallException  if the argument is not of the expected type
         */
        protected function setMyProperty($value)
        {
            if (!empty($value) && !is_object($value) && !is_array($value))
            {
                throw new BadMethodCallException("Argument 'src' of event {$this->name} must be empty, object or array");
            }

            return $value;
        }
    }
```

Why have we introduced this name class over parameters? Well it makes it
easier to introduce custom setters and getters for properties -
currently a plugin can either completely change a property as it wants -
for a components there's no way of imposing any limitations.
Additionally it makes it much easier for developers to add and remove
parameters in an event without having major b/c issues (as you are now
calling defined methods and are not subject to a property being the 2nd
argument of your function).

#### How to achieve maximum compatibility with Joomla 3

If you want to trigger an event in a similar way to the removed J3.x
JEventDispatcher then you use code like this:

```php
    $results = \Joomla\CMS\Factory::getApplication()->triggerEvent( '',  );
```

It is important to note that the parameters have to be in an array. The
plugin function itself will get the parameters as an Event object if it
implements the SubscriberInterface and as individual values if it does
not, but this method will always return an array that the plugin
returns.

Note that if ANY plugin in a group doesn't implement the
SubscriberInterface then the result property (as both a named parameter
and result from a plugin) is used as a special property and cannot be
used.
