<!-- Filename: J4.x:Selecting_data_using_JDatabase / Display title: Selecting data using JDatabase -->

Joomla!  4.x Version Note

Note many examples online do not use the prepared statements methods
that have been introduced with Joomla 4.x, please do not use these old
APIs for new projects, because they result in massive security issues if
user input is not strictly escaped.

This tutorial is split into two independent parts:

- Inserting, updating and removing data from the database.
- Selecting data from one or more tables and retrieving it in a variety
  of different forms

This section of the documentation looks at selecting data from a
database table and retrieving it in a variety of formats. To see the
other part [click
here](https://docs.joomla.org/J4.x:Inserting_Updating_and_Removing_data_using_JDatabase "Special:MyLanguage/J4.x:Inserting Updating and Removing data using JDatabase")

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

Since Joomla introduced support for a variety of database types in
Joomla 1.6 - the recommended way of building database queries is through
"query chaining" (although string queries will always be supported).

Query chaining refers to a method of connecting a number of methods, one
after the other, with each method returning an object that can support
the next method, improving readability and simplifying code.

To obtain a new instance of the \Joomla\Database\DatabaseQuery class we
use the \Joomla\Database\DatabaseDriver getQuery method:

    use Joomla\CMS\Factory;

    $db = Factory::getDbo();

    $query = $db->getQuery(true);

The \Joomla\Database\DatabaseDriver::getQuery takes an optional
argument, \$new, which can be true or false (the default being false).

To query our data source we can call a number of
\Joomla\Database\DatabaseQuery methods; these methods encapsulate the
data source's query language (in most cases SQL), hiding query-specific
syntax from the developer and increasing the portability of the
developer's source code.

Some of the more frequently used methods include; select, from, join,
where and order. There are also methods such as insert, update and
delete for modifying records in the data store. By chaining these and
other method calls, you can create almost any query against your data
store without compromising portability of your code.

## Selecting Records from a Single Table

Below is an example of creating a database query using the
`\Joomla\Database\DatabaseQuery` class. Using the select, from, where
and order methods, we can create queries which are flexible, easily
readable and portable:

    use Joomla\CMS\Factory;

    // Get a db connection.
    $db = Factory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Select all records from the user profile table where key begins with "custom.".
    // Order it by the ordering field.
    $query->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']));
    $query->from($db->quoteName('#__user_profiles'));
    $query->where($db->quoteName('profile_key') . ' LIKE :profile_key');
    $query->order($db->quoteName('ordering') . ' ASC');

    // bind value for prepared statements
    $query->bind(':profile_key', 'custom.%');

    // Reset the query using our newly populated query object.
    $db->setQuery($query);

    // Load the results as a list of stdClass objects (see later for more options on retrieving data).
    $results = $db->loadObjectList();

The query can also be chained to simplify further:

    $query
        ->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']))
        ->from($db->quoteName('#__user_profiles'))
        ->where($db->quoteName('profile_key') . ' LIKE :profile_key')
        ->order($db->quoteName('ordering') . ' ASC')
        ->bind(':profile_key', 'custom.%');

Chaining can become useful when queries become longer and more complex.

Grouping can be achieved simply too. The following query would count the
number of articles in each category.

    $query
        ->select( ['catid', 'COUNT(*)'] )
        ->from($db->quoteName('#__content'))
        ->group($db->quoteName('catid'));

A limit can be set to a query using "setLimit". For example in the
following query, it would return up to 10 records.

    $query
        ->select($db->quoteName(['user_id', 'profile_key', 'profile_value', 'ordering']))
        ->from($db->quoteName('#__user_profiles'))
        ->setLimit('10');

## Selecting Records from Multiple Tables

Using the \Joomla\Database\DatabaseQuery's <a
href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#join"
class="external text" target="_blank" rel="noreferrer noopener">join</a>
methods, we can select records from multiple related tables. The generic
"join" method takes two arguments; the join "type" (inner, outer, left,
right) and the join condition. In the following example you will notice
that we can use all of the keywords we would normally use if we were
writing a native SQL query, including the AS keyword for aliasing tables
and the ON keyword for creating relationships between tables. Also note
that the table alias is used in all methods which reference table
columns (I.e. select, where, order).

    use Joomla\CMS\Factory;

    // Get a db connection.
    $db = Factory::getDbo();

    // Create a new query object.
    $query = $db->getQuery(true);

    // Select all articles for users who have a username which starts with 'a'.
    // Order it by the created date.
    // Note by putting 'a' as a second parameter will generate `#__content` AS `a`
    $query
        ->select(['a.*', 'b.username', 'b.name'])
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

    // Reset the query using our newly populated query object.
    $db->setQuery($query);

    // Load the results as a list of stdClass objects (see later for more options on retrieving data).
    $results = $db->loadObjectList();

The join method above enables us to query both the content and user
tables, retrieving articles with their author details. There are also
convenience methods for joins:

- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_innerJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">innerJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_leftJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">leftJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_rightJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">rightJoin()</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html#method_outerJoin"
  class="external text" target="_blank"
  rel="noreferrer noopener">outerJoin()</a>

We can use multiple joins to query across more than two tables:

    $query
        ->select(array('a.*', 'b.username', 'b.name', 'c.*', 'd.*'))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->join('LEFT', $db->quoteName('#__user_profiles', 'c') . ' ON (' . $db->quoteName('b.id') . ' = ' . $db->quoteName('c.user_id') . ')')
        ->join('RIGHT', $db->quoteName('#__categories', 'd') . ' ON (' . $db->quoteName('a.catid') . ' = ' . $db->quoteName('d.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

Notice how chaining makes the source code much more readable for these
longer queries.

In some cases, you will also need to use the AS clause when selecting
items to avoid column name conflicts. In this case, multiple select
statements can be chained in conjunction with using the second parameter
of \$db-\>quoteName.

    $query
        ->select('a.*')
        ->select($db->quoteName('b.username', 'username'))
        ->select($db->quoteName('b.name', 'name'))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b'), $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id'))
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

A second array can also be used as the second parameter of the select
statement to populate the values of the AS clause. Remember to include
nulls in the second array to correspond to columns in the first array
that you don't want to use the AS clause for:

    $query
        ->select(['a.*'])
        ->select($db->quoteName(array('b.username', 'b.name'), ['username', 'name']))
        ->from($db->quoteName('#__content', 'a'))
        ->join('INNER', $db->quoteName('#__users', 'b') . ' ON (' . $db->quoteName('a.created_by') . ' = ' . $db->quoteName('b.id') . ')')
        ->where($db->quoteName('b.username') . ' LIKE :username')
        ->order($db->quoteName('a.created') . ' DESC')
        ->bind(':username', 'a%');

## Using prepared statements

With Joomla! 4.0 we have [moved all queries to use prepared
statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "J4.x:Moving Joomla To Prepared Statements").
For easier use of prepared statements we introduced some helper
functions and allow to use arrays in several function calls. Simple
query with prepared statements.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

You see that we don't add the **\$credentials\['username'\]** directly
to the query, instead we add the placeholder **:username** into the
query and **bind** the variable to the query. When we bind a variable to
a query we don't need to escape nor to quote it. Beware that binding a
variable will always be a reference. A nice side effect of this, is that
you can manipulate the query in a loop.

    $listOfUsernames = [ 'admin', 'user1' ];

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $username);

    foreach($listOfUsernames as $name)
    {
      $username = $name;
      $this->db->setQuery($query);
      $user = $this->db->loadObject();
      print_r($user);
    }

In the loop we set the previously bound \$username variable with the
\$name variable from the loop, then we have to set the query again
(because Joomla reset the database driver after query execution which is
only true for load functions). The result of this will be multiple
queries with different username values. We can use arrays to add
multiple variables at once.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->where($this->db->quoteName('id') . ' = :id')
        ->bind([':username', ':id'], [$credentials['username'], 42], [Joomla\Database\ParameterType::STRING, Joomla\Database\ParameterType::INTEGER]);

We add **username** and **id** as bind parameter and set the correct
ParameterType for each variable. It's also possible to use one variable
for all bind values and ParameterTypes.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->where($this->db->quoteName('password') . ' = :password')
        ->bind([':username', ':password], $credentials['username']);

The parameter :username and :password get set to the same value and the
default ParameterType. The function **whereIn()** and **whereNotIn()**
always use prepared statements, internal these functions uses the
**bindArray** function. It can be used to bind an array of variables
without specifying the placeholder.

    $userids = [1,2,3,4];

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'));

    $parameterNames = $query->bindArray($userids);

    $query->where($this->db->quoteName('id') . ' IN (' . implode(',', $parameterNames) . ')');

The **bindArray** function returns an array of placeholders. The index
is unique for the whole query.

    $placeholders = [
      ':preparedArray1',
      ':preparedArray2',
      ':preparedArray3',
      ':preparedArray4'
    ];

## Query Results

The database class contains many methods for working with a query's
result set.

### Single Value Result

#### loadResult()

Use **loadResult()** when you expect just a single value back from your
database query.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

This is often the result of a 'count' query to get a number of records:

    use Joomla\CMS\Factory;
    $db = Factory::getDbo();
    $query = $db->getQuery(true);
    $query->select('COUNT(*)');
    $query->from($db->quoteName('#__my_table'));
    $query->where($db->quoteName('name')." = :value");
    $query->bind('value', $value)

    // Reset the query using our newly populated query object.
    $db->setQuery($query);
    $count = $db->loadResult();

or where you are just looking for a single field from a single row of
the table (or possibly a single field from the first row returned).

    use Joomla\CMS\Factory;
    $db = Factory::getDbo();
    $query = $db->getQuery(true);
    $query->select('field_name');
    $query->from($db->quoteName('#__my_table'));
    $query->where($db->quoteName('some_name')." = :value");
    $query->bind(':value', $some_value);

    $db->setQuery($query);
    $result = $db->loadResult();

### Single Row Results

Each of these results functions will return a single record from the
database even though there may be several records that meet the criteria
that you have set. To get more records you need to call the function
again.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

#### loadRow()

`loadRow()` returns an indexed array from a single record in the table:

    . . .
    $db->setQuery($query);
    $row = $db->loadRow();
    print_r($row);

will give:

    Array ( [0] => 1, [1] => John Smith, [2] => johnsmith@domain.example, [3] => johnsmith ) 

You can access the individual values by using:

    $row['index'] // e.g. $row['2']

Notes:

1.  The array indices are numeric starting from zero.
2.  Whilst you can repeat the call to get further rows, one of the
    functions that returns multiple rows might be more useful.

#### loadAssoc()

`loadAssoc()` returns an associated array from a single record in the
table:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssoc();
    print_r($row);

will give:

    Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith )

You can access the individual values by using:

    $row['name'] // e.g. $row['email']

Notes:

1.  Whilst you can repeat the call to get further rows, one of the
    functions that returns multiple rows might be more useful.

#### loadObject()

loadObject returns a PHP object from a single record in the table:

    . . .
    $db->setQuery($query);
    $result = $db->loadObject();
    print_r($result);

will give:

    stdClass Object ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith )

You can access the individual values by using:

    $result->index // e.g. $result->email

Notes:

1.  Whilst you can repeat the call to get further rows, one of the
    functions that returns multiple rows might be more useful.

### Single Column Results

Each of these results functions will return a single column from the
database.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

#### loadColumn()

`loadColumn()` returns an indexed array from a single column in the
table:

    $query->select('name'));
          ->from . . .";
    . . .
    $db->setQuery($query);
    $column= $db->loadColumn();
    print_r($column);

will give:

    Array ( [0] => John Smith, [1] => Magda Hellman, [2] => Yvonne de Gaulle )

You can access the individual values by using:

    $column['index'] // e.g. $column['2']

Notes:

1.  The array indices are numeric starting from zero.
2.  `loadColumn()` is equivalent to loadColumn(0).

#### loadColumn(\$index)

loadColumn(\$index) returns an indexed array from a single column in the
table:

    $query->select(array('name', 'email', 'username'));
          ->from . . .";
    . . .
    $db->setQuery($query);
    $column= $db->loadColumn(1);
    print_r($column);

will give:

    Array ( [0] => johnsmith@domain.example, [1] => magda_h@domain.example, [2] => ydg@domain.example )

You can access the individual values by using:

    $column['index'] // e.g. $column['2']

loadColumn(\$index) allows you to iterate through a series of columns in
the results

    . . .
    $db->setQuery($query);
    for ( $i = 0; $i <= 2; $i++ ) {
      $column = $db->loadColumn($i);
      print_r($column);
    }

will give:

    Array ( [0] => John Smith, [1] => Magda Hellman, [2] => Yvonne de Gaulle ),
    Array ( [0] => johnsmith@domain.example, [1] => magda_h@domain.example, [2] => ydg@domain.example ),
    Array ( [0] => johnsmith, [1] => magdah, [2] => ydegaulle )

Notes:

1.  The array indices are numeric starting from zero.

### Multi-Row Results

Each of these results functions will return multiple records from the
database.

|     |                  |                          |           |
|-----|------------------|--------------------------|-----------|
| id  | name             | email                    | username  |
| 1   | John Smith       | johnsmith@domain.example | johnsmith |
| 2   | Magda Hellman    | magda_h@domain.example   | magdah    |
| 3   | Yvonne de Gaulle | ydg@domain.example       | ydegaulle |

#### loadRowList()

`loadRowList()` returns an indexed array of indexed arrays from the
table records returned by the query:

    . . .
    $db->setQuery($query);
    $row = $db->loadRowList();
    print_r($row);

will give (with line breaks added for clarity):

    Array ( 
    [0] => Array ( [0] => 1, [1] => John Smith, [2] => johnsmith@domain.example, [3] => johnsmith ), 
    [1] => Array ( [0] => 2, [1] => Magda Hellman, [2] => magda_h@domain.example, [3] => magdah ), 
    [2] => Array ( [0] => 3, [1] => Yvonne de Gaulle, [2] => ydg@domain.example, [3] => ydegaulle ) 
    )

You can access the individual rows by using:

    $row['index'] // e.g. $row['2']

and you can access the individual values by using:

    $row['index']['index'] // e.g. $row['2']['3']

Notes:

1.  The array indices are numeric starting from zero.

#### loadAssocList()

`loadAssocList()` returns an indexed array of associated arrays from the
table records returned by the query:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList();
    print_r($row);

will give (with line breaks added for clarity):

    Array ( 
    [0] => Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [1] => Array ( [id] => 2, [name] => Magda Hellman, [email] => magda_h@domain.example, [username] => magdah ), 
    [2] => Array ( [id] => 3, [name] => Yvonne de Gaulle, [email] => ydg@domain.example, [username] => ydegaulle ) 
    ) 

You can access the individual rows by using:

    $row['index'] // e.g. $row['2']

and you can access the individual values by using:

    $row['index']['column_name'] // e.g. $row['2']['email']

#### loadAssocList(\$key)

`loadAssocList('key')` returns an associated array - indexed on 'key' -
of associated arrays from the table records returned by the query:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList('username');
    print_r($row);

will give (with line breaks added for clarity):

    Array ( 
    [johnsmith] => Array ( [id] => 1, [name] => John Smith, [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [magdah] => Array ( [id] => 2, [name] => Magda Hellman, [email] => magda_h@domain.example, [username] => magdah ), 
    [ydegaulle] => Array ( [id] => 3, [name] => Yvonne de Gaulle, [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

You can access the individual rows by using:

    $row['key_value'] // e.g. $row['johnsmith']

and you can access the individual values by using:

    $row['key_value']['column_name'] // e.g. $row['johnsmith']['email']

Note: Key must be a valid column name from the table; it does not have
to be an Index or a Primary Key. But if it does not have a unique value
you may not be able to retrieve results reliably.

#### loadAssocList(\$key, \$column)

`loadAssocList('key', 'column')` returns an associative array, indexed
on 'key', of values from the column named 'column' returned by the
query:

    . . .
    $db->setQuery($query);
    $row = $db->loadAssocList('id', 'username');
    print_r($row);

will give (with line breaks added for clarity):

    Array ( 
    [1] => John Smith, 
    [2] => Magda Hellman, 
    [3] => Yvonne de Gaulle,
    )

Note: Key must be a valid column name from the table; it does not have
to be an Index or a Primary Key. But if it does not have a unique value
you may not be able to retrieve results reliably.

#### loadObjectList()

`loadObjectList()` returns an indexed array of PHP objects from the
table records returned by the query:

    . . .
    $db->setQuery($query);
    $row = $db->loadObjectList();
    print_r($row);

will give (with line breaks added for clarity):

    Array ( 
    [0] => stdClass Object ( [id] => 1, [name] => John Smith, 
        [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [1] => stdClass Object ( [id] => 2, [name] => Magda Hellman, 
        [email] => magda_h@domain.example, [username] => magdah ), 
    [2] => stdClass Object ( [id] => 3, [name] => Yvonne de Gaulle, 
        [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

You can access the individual rows by using:

    $row['index'] // e.g. $row['2']

and you can access the individual values by using:

    $row['index']->name // e.g. $row['2']->email

#### loadObjectList(\$key)

`loadObjectList('key')` returns an associated array - indexed on 'key' -
of objects from the table records returned by the query:

    . . .
    $db->setQuery($query);
    $row = $db->loadObjectList('username');
    print_r($row);

will give (with line breaks added for clarity):

    Array ( 
    [johnsmith] => stdClass Object ( [id] => 1, [name] => John Smith, 
        [email] => johnsmith@domain.example, [username] => johnsmith ), 
    [magdah] => stdClass Object ( [id] => 2, [name] => Magda Hellman, 
        [email] => magda_h@domain.example, [username] => magdah ), 
    [ydegaulle] => stdClass Object ( [id] => 3, [name] => Yvonne de Gaulle, 
        [email] => ydg@domain.example, [username] => ydegaulle ) 
    )

You can access the individual rows by using:

    $row['key_value'] // e.g. $row['johnsmith']

and you can access the individual values by using:

    $row['key_value']->column_name // e.g. $row['johnsmith']->email

Note: Key must be a valid column name from the table; it does not have
to be an Index or a Primary Key. But if it does not have a unique value
you may not be able to retrieve results reliably.

### Miscellaneous Result Set Methods

#### getNumRows()

`getNumRows()` will return the number of result rows found by the last
SELECT or SHOW query and waiting to be read. To get a result from
getNumRows() you have to run it **after** the query and **before** you
have retrieved any results. To retrieve the number of rows affected by a
INSERT, UPDATE, REPLACE or DELETE query, use getAffectedRows().

    . . .
    $db->setQuery($query);
    $db->execute();
    $num_rows = $db->getNumRows();
    print_r($num_rows);
    $result = $db->loadRowList();

will return

    3

Note: getNumRows() is only valid for statements like SELECT or SHOW that
return an actual result set. If you run getNumRows() after
loadRowList() - or any other retrieval method - you will get a PHP
Warning:

    Warning: mysql_num_rows(): 80 is not a valid MySQL result resource 
    in libraries\joomla\database\database\mysql.php on line 344

### See also

- [Inserting, Updating and Removing data using
  JDatabase](https://docs.joomla.org/J4.x:Inserting,_Updating_and_Removing_data_using_JDatabase "Special:MyLanguage/J4.x:Inserting, Updating and Removing data using JDatabase")
- [Moving Joomla To Prepared
  Statements](https://docs.joomla.org/J4.x:Moving_Joomla_To_Prepared_Statements "Special:MyLanguage/J4.x:Moving Joomla To Prepared Statements")
- [Using the union methods in database
  queries](https://docs.joomla.org/Using_the_union_methods_in_database_queries "Special:MyLanguage/Using the union methods in database queries")
  (Joomla! 3.3+)
- <a href="https://php.net/manual/en/pdo.prepared-statements.php"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">The PHP Manual on Prepared
  statements</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Framework API</a>
