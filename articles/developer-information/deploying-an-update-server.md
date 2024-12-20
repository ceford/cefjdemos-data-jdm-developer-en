<!-- Filename: Deploying_an_Update_Server / Display title: Update Servers -->

## Background

There is a good description of Update Servers in the Joomla! Programmers Documentation copied to [this site](jdocmanual?article=docus/install-update/update-server) or available in the [original source](https://manual.joomla.org/docs/building-extensions/install-update/update-server/).

## Troubleshooting

- **SQL update script is not executed during update.**

If the SQL update script (for example, in the folder
`sql/updates/mysql`) does not get executed during the update process, it
could be because there is no version number in the `#_schemas` table for
this extension *prior to the update*. This value is determined by the
last script name in the SQL updates folder. If this value is blank, no
SQL scripts will be executed during that update cycle. To make sure this
value is set correctly, make sure you have a SQL script in this folder
with its name as the version number (for example, 1.2.3.sql if the
version is 1.2.3). The file can be empty or just have a SQL comment
line. This should be done in the old version — the one before the
update. Alternatively, you can add this value to the `#_schemas` using a
SQL query.
