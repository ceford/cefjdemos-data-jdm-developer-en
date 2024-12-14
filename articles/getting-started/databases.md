<!-- Filename: J4.x:Developer:_Required_Software / Display title: Database Setup -->

## About MySQL and MariaDB

Newcomers may have the impression that MySQL and MariaDB are databases and may be confused when instructions say *create a new database*. In fact, both are Relational Database Management Systems (RDBMS) that can manage many individual databases. Your local test site may have many individual Joomla installations each with its own database. For example, you may wish to test your extension in the current Joomla release and separately in an upcoming release candidate.

The following screenshot shows part of a list of over 30 databases created for testing various Joomla installations and extensions projects.

![Phypadmin screenshot of list of databases](../../../en/images/getting-started/phpmyadmin-databases.png)

A side note: the collations are mostly utf8mb4_0900_ai_ci:

- utf8mb4 means that each character is stored as a maximum of 4 bytes in the UTF-8 encoding scheme.
- 0900 refers to the Unicode Collation Algorithm version.
- ai refers accent insensitivity, there is no difference between e, è, é, ê and ë when sorting.
- ci refers to case insensitivity, there is no difference between p and P when sorting.

Individual tables and columns may have a different collation. Joomla tables typically have utf8mb4_unicode_ci collation.

## Database Setup with phpMyAdmin

Before installing Joomla you need an empty database ready to be populated. You also need a database user.

### Create a Database

- **Start phpMyAdmin** Enter localhost/phpmyadmin in your browser url bar.
- **Login** Depending on how it was installed, the username will be root or admin and there may or may not be a password.
- Select **Databases** from the phpMyAdmin Home page top menu.
- In **Create Database** enter a short name in place of the **Database name** hint, for example, jtest.
- Select **utf8mb4_0900_ai_ci** from the Collation drop-down list.
- Select **Create** to create the database.

That will take you to an empty database. At the top there is a message: *No tables found in database.*

### Create a User

- Select the **Home** icon at the top left of phpMyAdmin to go to the Home page.
- Select **User Accounts** from the Home top menu.
- Select **Add User Account** from the New panel beneath the list of current users (if any).
- Enter a **User name** which can be a short name you will need later during Joomla installation. Example: jtest. You can use this same user for other databases created later!
- Select **Local** from the Hostname field list. It will set localhost in the adjacent value field.
- Use the **Generate** button to generate a password. You need to copy the generated value and paste it into a text editor for use later. You do not need to remember it long term as it will be stored as plain text in the Joomla configuration.php file. Example: 8t8mGQq.gw\[\]8lp(
- **Save** skip the Database for user and Global privileges sections of the form. The **Go** (Save) button is at the bottom.

### Assign Database Permissions to the User

- In the **User Accounts** page select the user (jtest), if necessary via Home / User accounts / User name.
- Select **Database** near the top of the page. That will show a list of databases to which this user has been granted access permissions and, at the bottom, a list of databases to which the user has not been granted access.
- **Select** the database to which assess is to be granted and select the **Go** button.
- **Database specific privileges** Select **Check all** and then **Go** to save.

That is it! You can now logout of phpMyAdmin. Did you forget to write down the database user password? Go back and edit the User account you created to change it. If you use the same database user for multiple Joomla databases you will find the password in a Joomla configuration.php file.
