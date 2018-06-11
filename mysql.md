# Mysql

#### install

* for server

  * `sudo apt install mysql-server`

* for client
  * `sudo apt install mysql-client`



#### login

`mysql -u USER -p -h HOSTNAME`

#### list databases

`SHOW DATABASES;`

#### Create DB

`CREATE DATABASE db_name;`

#### Create User

`CREATE USER db_user@localhost;`

#### Set password to user

`SET PASSWORD FOR db_user@localhost= PASSWORD("password_of_username");`

#### grant privilege of a DB to a user

`GRANT ALL PRIVILEGES ON db_name.* TO db_user@localhost IDENTIFIED BY 'password_of_username';`

#### grant privilege of a namespace to a user

``grant all privileges on `namespace\_%`.* to 'db_user'@'localhost' ;``

#### Delete database

`DROP DATABASE db_name`

#### Create backup

`mysqldump -u DB_USER -p DB_NAME > filename.sql`

#### restore backup

`mysql -u root -p -h localhost DB_NAME < filename.sql`









