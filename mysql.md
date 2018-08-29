# Mysql

## install

* for server
  * `sudo apt install mysql-server`
* for client
  * `sudo apt install mysql-client`

## login

`mysql -u USER -p -h HOSTNAME`

## list databases

`SHOW DATABASES;`

## Create DB

`CREATE DATABASE db_name;`

## Create User

`CREATE USER db_user@localhost;`

## Set password to user

`SET PASSWORD FOR db_user@localhost= PASSWORD("password_of_username");`

## grant privilege of a DB to a user

`GRANT ALL PRIVILEGES ON db_name.* TO db_user@localhost IDENTIFIED BY 'password_of_username';`

## grant privilege of a namespace to a user

``grant all privileges on `namespace\_%`.* to 'db_user'@'localhost' ;``

## Delete database

`DROP DATABASE db_name`

## Create backup

`mysqldump -u DB_USER -p DB_NAME > filename.sql`

## restore backup

`mysql -u root -p -h localhost DB_NAME < filename.sql`

## Create tabale

```text
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] table(
   key type(size) NOT NULL PRIMARY KEY AUTO_INCREMENT,
   c1 type(size) NOT NULL,
   c2 type(size) NULL,
   ...
);
```

## Add a new column into a table

`ALTER TABLE table ADD [COLUMN];`

## Drop an existing column in a table

`ALTER TABLE table DROP [COLUMN];`

## Add index with a specific name to a table on a column

`ALTER TABLE table ADD INDEX [name](column, ...);`

## Add primary key into a table

`ALTER TABLE table ADD PRIMARY KEY (column,...)`

## Remove primary key from a table

`ALTER TABLE table DROP PRIMARY KEY`

## Deleting table structure and data permanently

`DROP TABLE [IF EXISTS] table [, name2, ...] [RESTRICT | CASCADE]`

## Creating an index with the specified name on a table

`CREATE [UNIQUE|FULLTEXT] INDEX index_name ON table (column,...)`

## Removing a specified index from table

`DROP INDEX index_name`

## Query all data from a table

`SELECT * FROM table`

## Query specified data which is shown in the column list from a table

`SELECT column, column2 FROM table;`

## Query unique records

`SELECT DISTINCT (column) FROM table;`

## Query data with a filter using a WHERE clause

`SELECT * FROM table WHERE condition;`

