---
layout: post
title:  MySQL vs. PostgreSQL
author: Zhuyi Xue
tags: database,mysql,postgresql
---

Here is a list of brief comparison of common commands used in MySQL and
PostgreSQL.

Note: commands with a ">" prefix means it should be executed after connected to
db server. Otherwise, it's a shell command.  Data created by the two could be
in a directory  like `/usr/local/var/`.

Software versions:

* MySQL (5.5.28)
* PostgreSQL (9.2.1)

#### Get help:

    >\h # OR >\? OR >help [COMAND]
    # vs.
    >\h # OR >\?

#### Init a database:

    mysql_install_db --datadir=<DIR> --basedir=<DIR> --user=<USER>
    # vs.
    initdb <DIR> -E utf8

#### Start a server:

    mysql.server start # OR service mysqld start
    # vs.
	pg_ctl -D <DIR> -l <FILE> start

#### Stop server
	
    mysql.server stop # OR service mysqld stop
	# vs.
	pg_ctl -D <DIR> stop [-s -m fast]

#### Connect to server (there are variants, but with the same effect)

    mysql -h ####23.123.123.123 --port 3306 \
	      --user=USER --password=PASSWD --database DBNAME
	# vs.
	psql  -h ####23.123.123.123 --port 5432 \
          --username USER --pasword PASSWD --dbname DBNAME [sslmode=requir]

#### List all databases

    >SHOW DATABASES;
    # vs.
	>\l[+]
	
#### Show the current database

    >select database();
	# vs.
	>select current_database();
	
#### Create a database

    >CREATE DATABASE <DBNAME>
	# vs.
	# same as MySQL. Besides, can also use createdb command in shell.
	
#### Drop a database

    >DROP DATABASE <DBNAME>
	# vs.
	same as MySQL. Besides, can also use dropdb command in shell.
	
#### Connect to a new database

    >USE (\u) [DBNAME];
	# vs.
	>\c[onnect] [DBNAME|- USER|- HOST|- PORT|-]
	
#### List all tables in the current database

    >SHOW TABLES;
    # vs.
	>\d[+]
	
#### Show the table schema

    >DESCRIBE <TABNAME>; 
	# or 
	>SHOW CREATE TABLE <TABNAME>; 
	# or 
	>SHOW COLUMNS FROM <TABNAME>;
    # vs.
	>\d[S+] <TABNAME>

#### Show all users

    SELECT User from mysql.user;
	# vs.
	SELECT * FROM pg_user;


