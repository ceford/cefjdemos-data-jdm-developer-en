<!-- Filename: API_Guides / Display title: API Guides -->

This page contains an index to the set of Joomla API Guides. These
guides aim to help you understand how to use these Joomla functions in
your own Joomla extensions.

Each API guide includes sample code which you can copy and install in
your own development environment. Generally these code samples are
written to be included and installed as Joomla module, so if you're not
familiar with Joomla module development you may find going through the
short series [Creating a Simple
Module](https://docs.joomla.org/Creating_a_simple_module "Creating a simple module")
useful.

Around Joomla 3.8 the Joomla development team began changing the naming
convention of Joomla classes to use namespaces, so that for example
JFactory changed to Factory in the Joomla\CMS namespace. As you read
existing Joomla code and documentation you may find the classes
following either the new or the old naming standard. You can find the
mapping between the two naming conventions in the
`libraries/classmap.php` file in your Joomla instance.

- The base Application classes and their hierarchy and purposes is
  described in [Understanding the Application
  classes](https://docs.joomla.org/J3.x:Understanding_the_Application_classes "J3.x:Understanding the Application classes")
- Ajax handling within Joomla Components is described in [JSON Responses
  with
  JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson").
  Ajax can also be used within Joomla Modules and Plugins as described
  in [Using Joomla Ajax
  Interface](https://docs.joomla.org/Using_Joomla_Ajax_Interface "Using Joomla Ajax Interface").
- [Cache](https://docs.joomla.org/Cache_Basic_API_Guide "Cache Basic API Guide") -
  how to use callback cache within your code.
- [Categories](https://docs.joomla.org/Categories_and_CategoryNodes_API_Guide "Categories and CategoryNodes API Guide")
  Using the `Categories` and `CategoryNode` classes to access data
  relating to Joomla categories
- CSS can be added as described in
  <a href="https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page"
  class="mw-redirect" title="Adding JavaScript and CSS to the page">Adding
  JavaScript and CSS to the page</a>
- Database / JDatabase. There are two API guides, covering [Selecting
  data using
  JDatabase](https://docs.joomla.org/Selecting_data_using_JDatabase "Selecting data using JDatabase")
  and [Inserting, Updating and Removing data using
  JDatabase](https://docs.joomla.org/Inserting,_Updating_and_Removing_data_using_JDatabase "Inserting, Updating and Removing data using JDatabase")
- [Date/JDate](https://docs.joomla.org/How_to_use_JDate "How to use JDate")
  is Joomla's Date class.
- Files and Folders. See [How to use the filesystem
  package](https://docs.joomla.org/How_to_use_the_filesystem_package "How to use the filesystem package").
- Form / JForm. There is a [Basic
  guide](https://docs.joomla.org/Basic_form_guide "Basic form guide") to
  using the Joomla Form API and incorporating forms within a Joomla
  component, and also a more [Advanced form
  guide](https://docs.joomla.org/Advanced_form_guide "Advanced form guide")
  covering more advanced aspects of the APIs.
- FormField / JFormField. This class and related classes such as
  JFormFieldList which inherit from it are primarily useful for defining
  custom form fields, as described in [Creating a custom form field
  type](https://docs.joomla.org/Creating_a_custom_form_field_type "Creating a custom form field type").
- [Input /
  JInput](https://docs.joomla.org/Retrieving_request_data_using_JInput "Retrieving request data using JInput")
  Using the `Input` class to obtain the values of parameters in HTTP GET
  and POST requests
- JavaScript can be added as described in
  <a href="https://docs.joomla.org/Adding_JavaScript_and_CSS_to_the_page"
  class="mw-redirect" title="Adding JavaScript and CSS to the page">Adding
  JavaScript and CSS to the page</a>
- Using Joomla Layouts is described in [Sharing layouts across views or
  extensions with
  JLayout](https://docs.joomla.org/J3.x:Sharing_layouts_across_views_or_extensions_with_JLayout "J3.x:Sharing layouts across views or extensions with JLayout").
  The flexibility was increased in Joomla 3.2, as described in [JLayout
  Improvements for
  Joomla!](https://docs.joomla.org/J3.x:JLayout_Improvements_for_Joomla! "J3.x:JLayout Improvements for Joomla!").
- [Log / JLog](https://docs.joomla.org/Using_JLog "Using JLog") Log
  messages (eg error messages, debug messages) to a log file, and
  optionally to the debug console
- [Menu and
  Menuitems](https://docs.joomla.org/Menu_and_Menuitems_API_Guide "Menu and Menuitems API Guide")
- [Nested
  Sets](https://docs.joomla.org/Using_nested_sets "Using nested sets"),
  which enable a tree hierarchy to be implemented in the database table,
  are used by Joomla menus, articles, categories, etc.
- <a href="https://github.com/joomla-framework/registry"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Registry/JRegistry</a> is a utility
  class which is very useful for handling PHP arrays, converting to
  JSON, etc.
- [JResponseJson](https://docs.joomla.org/JSON_Responses_with_JResponseJson "JSON Responses with JResponseJson")
  supports responding in JSON format to Ajax requests.
- Route / JRoute see [URLs in
  Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- Table / JTable provides functionality for performing CRUD operations
  (and more) on database tables. The guide is split into a [Basic API
  Guide](https://docs.joomla.org/Table_Basic_API_Guide "Table Basic API Guide")
  and an [Advanced API
  Guide](https://docs.joomla.org/Table_Advanced_API_Guide "Table Advanced API Guide")
- The [Controllers](https://docs.joomla.org/Controllers "Controllers")
  (BaseController, AdminController, FormController, ApiController) are
  responsible for analysing the user's request, checking that the user
  is allowed to perform that action and determining how to satisfy the
  request.
- The [Models](https://docs.joomla.org/Models "Models") (BaseModel,
  BaseDatabaseModel, ItemModel, ListModel, FormModel, AdminModel)
  encapsulate the data used by the component. They are also responsible
  for updating the database where appropriate.
- The [Views](https://docs.joomla.org/Views "Views") (AbstractView,
  CategoriesView, CategoryFeedView, CategoryView, FormView, HtmlView,
  JsonApiView, JsonView, ListView) specify what should appear on the web
  page, and collates all the data necessary for outputting the HTTP
  response..
- [Tags](https://docs.joomla.org/Tags_API_Guide "Tags API Guide").
- Uri / JUri see [URLs in
  Joomla](https://docs.joomla.org/URLs_in_Joomla "URLs in Joomla")
- [User /
  JUser](https://docs.joomla.org/Accessing_the_current_user_object "Accessing the current user object")
  API related to the Joomla User.
