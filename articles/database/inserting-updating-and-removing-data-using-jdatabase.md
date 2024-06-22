<!-- Filename: J4.x:Inserting_Updating_and_Removing_data_using_JDatabase / Display title: Inserting Updating and Removing data using JDatabase -->

Joomla!  4.x Joomla!  3.x Joomla!  2.5 Version Note

Note many examples online do not use the prepared statements methods
that have been introduced with Joomla 4.x, please do not use these old
APIs for new projects, because they result in massive security issues if
user input is not strictly escaped.

This tutorial is split into two independent parts:

- Inserting, updating and removing data from the database.
- Selecting data from one or more tables and retrieving it in a variety
  of different forms.

This section of the documentation looks at inserting, updating and
removing data from a database table. To see the other part [click
here](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")

## Introduction

Joomla provides a sophisticated database abstraction layer to simplify
the usage for third party developers. New versions of the Joomla
Platform API provide additional functionality which extends the database
layer further, and includes features such as connectors to a greater
variety of database servers and the query chaining to improve
readability of connection code and simplify SQL coding.

Joomla can use different kinds of SQL database systems and run in a
variety of environments with different table-prefixes. In addition to
these functions, the class automatically creates the database
connection. Besides instantiating the object you need just two lines of
code to get a result from the database in a variety of formats. Using
the Joomla database layer ensures a maximum of compatibility and
flexibility for your extension.

## The Query

Joomla's database querying has changed since the new Joomla Framework
was introduced "query chaining" is now the recommended method for
building database queries (although string queries are still supported).

Query chaining refers to a method of connecting a number of methods, one
after the other, with each method returning an object that can support
the next method, improving readability and simplifying code.

To obtain a new instance of the JDatabaseQuery class we use the
JDatabaseDriver getQuery method:

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

The `JDatabaseDriver::getQuery` takes an optional argument, \$new, which
can be true or false (the default being false).

To query our data source we can call a number of JDatabaseQuery methods;
these methods encapsulate the data source's query language (in most
cases SQL), hiding query-specific syntax from the developer and
increasing the portability of the developer's source code.

Some of the more frequently used methods include; select, from, join,
where and order. There are also methods such as insert, update and
delete for modifying records in the data store. By chaining these and
other method calls, you can create almost any query against your data
store without compromising portability of your code.

## Inserting a Record

### Using SQL

The JDatabaseQuery class provides a number of methods for building
insert queries, the most common being <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#insert"
class="external text" target="_blank"
rel="noreferrer noopener">insert</a>, <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#columns"
class="external text" target="_blank"
rel="noreferrer noopener">columns</a> and <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#values"
class="external text" target="_blank"
rel="noreferrer noopener">values</a>.

    // Get a db connection.
    $db = JFactory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Insert columns.
    $columns = array('user_id', 'profile_key', 'profile_value', 'ordering');

    // Prepare the insert query.
    $query
        ->insert($db->quoteName('#__user_profiles'))
        ->columns($db->quoteName($columns))
        ->values(':user_id, :profile_key, :profile_value, :ordering');

    // Bind values
    $query
        ->bind(':user_id', 1001, Joomla\Database\ParameterType::INTEGER)
        ->bind(':profile_key', 'custom.message')
        ->bind(':profile_value', 'Inserting a record using insert()')
        ->bind(':ordering', 1, Joomla\Database\ParameterType::INTEGER);

    // Set the query using our newly populated query object and execute it.
    $db->setQuery($query);

    $db->execute();

### Using an Object

The `JDatabaseDriver` class also provides a convenient method for saving
an object directly to the database allowing us to add a record to a
table without writing a single line of SQL.

    // Create and populate an object.
    $profile = new stdClass();
    $profile->user_id = 1001;
    $profile->profile_key='custom.message';
    $profile->profile_value='Inserting a record using insertObject()';
    $profile->ordering=1;

    // Insert the object into the user profile table.
    $result = JFactory::getDbo()->insertObject('#__user_profiles', $profile);

Notice here that we do not need to escape the table name; the
insertObject method does this for us.

The insertObject method will throw a error if there is a problem
inserting the record into the database table.

If you are providing a unique primary key value (as in the example
above), it is highly recommended that you select from the table by that
column value before attempting an insert.

If you are simply inserting the next row in your table (i.e. the
database generates a primary key value), you can specify the primary key
column-name as the third parameter of the insertObject() method and the
method will update the object with the newly generated primary key
value.

For example, given the following statement:

    $result = $dbconnect->insertObject('#__my_table', $object, 'primary_key');

after execution, \$object-\>primary_key will be updated with the newly
inserted row's primary key value.

**HINT:** Set \$object-\>primary_key to null or 0 (zero) before
inserting.

## Updating a Record

### Using SQL

The `JDatabaseQuery` class also provides methods for building update
queries, in particular <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#update"
class="external text" target="_blank"
rel="noreferrer noopener">update</a> and <a
href="http://api.joomla.org/11.4/Joomla-Platform/Database/JDatabaseQuery.html#set"
class="external text" target="_blank" rel="noreferrer noopener">set</a>.
We also reuse another method which we used when creating select
statements, the where method.

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

    // Fields to update.
    $fields = array(
        $db->quoteName('profile_value') . ' = :profile_value',
        $db->quoteName('ordering') . ' = :ordering'
    );

    // Conditions for which records should be updated.
    $conditions = array(
        $db->quoteName('user_id') . ' = :user_id', 
        $db->quoteName('profile_key') . ' = :profile_key'
    );

    $query->update($db->quoteName('#__user_profiles'))->set($fields)->where($conditions);

    $query
        ->bind(':profile_value', 'Updating custom message for user 1001.')
        ->bind(':ordering', 2, Joomla\Database\ParameterType::INTEGER)
        ->bind(':user_id', 42, Joomla\Database\ParameterType::INTEGER)   
        ->bind(':profile_key', 'custom.message');

    $db->setQuery($query);

    $result = $db->execute();

### Using an Object

Like `insertObject`, the JDatabaseDriver class provides a convenience
method for updating an object.

Below we will update our custom table with new values using an existing
id primary key:

    // Create an object for the record we are going to update.
    $object = new stdClass();

    // Must be a valid primary key value.
    $object->id = 1;
    $object->title = 'My Custom Record';
    $object->description = 'A custom record being updated in the database.';

    // Update their details in the users table using id as the primary key.
    $result = JFactory::getDbo()->updateObject('#__custom_table', $object, 'id');

Just like insertObject, updateObject takes care of escaping table names
for us.

The updateObject method will throw a error if there is a problem
updating the record into the database table.

We need to ensure that the record already exists before attempting to
update it, so we would probably add some kind of record check before
executing the updateObject method.

## Deleting a Record

Finally, there is also a delete method to remove records from the
database.

    $db = JFactory::getDbo();

    $query = $db->getQuery(true);

    // delete all custom keys for user 1001.
    $conditions = array(
        $db->quoteName('user_id') . ' = :user_id', 
        $db->quoteName('profile_key') . ' = :profile_key'
    );

    $query->delete($db->quoteName('#__user_profiles'));
    $query->where($conditions);

    $query
        ->bind(':user_id', 1001, Joomla\Database\ParameterType::INTEGER)
        ->bind(':profile_key', 'custom.%');

    $db->setQuery($query);

    $result = $db->execute();

### See also

- [Selecting data using
  JDatabase](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")
- [Moving Joomla To Prepared
  Statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements")
- [Using the union methods in database
  queries](https://docs.joomla.org/Using_the_union_methods_in_database_queries "Special:MyLanguage/Using the union methods in database queries")
  (Joomla! 3.3+)
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Framework API</a>
