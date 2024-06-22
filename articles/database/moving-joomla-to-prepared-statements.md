<!-- Filename: J4.x:Moving_Joomla_To_Prepared_Statements / Display title: Moving Joomla To Prepared Statements -->

Joomla!  <span class="small">≥ </span>4.0 version

In Joomla 4 you'll see the move of Joomla slowly across to using
prepared statements for all our queries. This article aims to set out
why and how we are doing this.

## Motivation

PHP 5.3 introduced the concept of prepared statements. In Joomla 4 we're
going to start taking advantage of them. The main advantage of prepared
statements is to reduce the exposure of a code base to SQL Injection
attacks by sending the query and the data separately. You can imagine
PHP Sending the query like the following

    Prepared Statements
    Query: SELECT foobar FROM bar WHERE foo = ?
    Data:  [? = 'bar']

And the database itself will do the hard work of quoting your statement.
Because the database is responsible for the quoting it reduces the
complexity of Joomla's code base and also reduces the chance of coding
errors introducing any bugs.

## Implementing Prepared Statements through JDatabaseDriver

Implementing prepared statements in Joomla is very simple and is
cross-platform. For example here's a simple
<img src="https://docs.joomla.org/images/4/4d/Compat_icon_3_x.png"
decoding="async" data-file-width="40" data-file-height="17" width="40"
height="17" alt="Joomla 3.x" /> query from Joomla's authentication
plugin.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . '=' . $this->db->quote($credentials['username']));

To convert it to a prepared statement we will instead substitute the
username for a named value and then bind our real data to the query.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id', 'password')))
        ->from($this->db->quoteName('#__users'))
        ->where($this->db->quoteName('username') . ' = :username')
        ->bind(':username', $credentials['username']);

As you can see whilst adding another line to the query we no longer have
to quote the data - we let our mysql/postgresql database handle this for
us giving us much easier to read and manage code.

Some functions in JDatabaseDriver will use prepared statements
automatically. For example <a
href="https://github.com/joomla-framework/database/blob/2.0-dev/src/DatabaseQuery.php#L1701"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">whereIn()</a> and <a
href="https://github.com/joomla-framework/database/blob/2.0-dev/src/DatabaseQuery.php#L1724"
class="external text" target="_blank"
rel="nofollow noreferrer noopener">whereNotIn()</a> will automatically
use the values and add prepared statements to the query.

    $query = $this->db->getQuery(true)
        ->select($this->db->quoteName(array('id, password')))
        ->from($this->db->quoteName('#__users'))
        ->whereIn($this->db->quoteName('id'), [ 1, 2, 3 ]);

This query will be converted to a sql prepared statement.

    SELECT 
      `id`, `password`
    FROM
      `#__users`
    WHERE
      `id` IN (
        :preparedArray1,
        :preparedArray2,
        :preparedArray3
      );

The placeholders :preparedArray1-3 will be fill with 1,2,3 on execute.

The following functions accept arrays to reduce function call overhead.

- bind()
- bindArray()
- whereIn()
- whereNotIn()

If possible you should use prepared statements.

## Further Reading on Prepared Statements

- [Selecting data using
  JDatabase](https://docs.joomla.org/J4.x:Selecting_data_using_JDatabase "Special:MyLanguage/J4.x:Selecting data using JDatabase")
- <a href="https://php.net/manual/en/pdo.prepared-statements.php"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">The PHP Manual on Prepared
  statements</a>
- <a href="https://github.com/joomla/joomla-cms/pull/25049/files"
  class="external text" target="_blank"
  rel="nofollow noreferrer noopener">Simple Pull Request Implementing a
  Prepared Statement in Joomla</a>
- <a
  href="https://api.joomla.org/framework-1/classes/Joomla.Database.DatabaseQuery.html"
  class="external text" target="_blank" rel="noreferrer noopener">Joomla
  Framework API</a>
