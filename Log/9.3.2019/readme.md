Here goes nothing!

 ### 1. Open mysql
 ```
cagood@benthos:~$ mysql
# Welcome to the MariaDB monitor.  Commands end with ; or \g.
# Your MariaDB connection id is 33
# Server version: 10.1.26-MariaDB-0+deb9u1 Debian 9.1

# Copyright (c) 2000, 2017, Oracle, MariaDB Corporation Ab and others.
 ```

So, it appears that we're running MariaDB - an open source fork of MySQL. It doesn't appear to be the most current. I don't want to upset anyone else's work on the server, so I'm going to work with it as-is.  

### 2. Where are data stored? - from bash shell
```
$ cd /etc/mysql/mariadb.conf.d/
$ less my.cnf

#...datadir         = /var/lib/mysql
```
There's a ton of useful information in this file/directory. 

### 3. Where are data stored? - from MariaDB shell
```
> show databases;

+--------------------+
| Database           |
+--------------------+
| information_schema |
+--------------------+
1 row in set (0.00 sec)
```

Cool. My understanding is that information_schema is a standard MySQL datastructure that self-populates with metadata from user-created dbs. Let's look around. 
```
# Use a database
> use information_schema;

# Show contents of db
> show tables;

# Show contents of a given table
> select * from databasetablename;

# Summarize contents of a given table
> describe databasetablename;
```
Each of these commands shows various levels of the data contained in information_schema. 

### 4. Recovering the admin password & creating a user

So this wasn't too bad. See [this article](https://www.liberiangeek.net/2014/10/reset-root-password-mariadb-centos-7/).

I reset the admin password (the original installer had no idea what I was talking about) using the steps in the above article. 

Now, to create a profile:
```
# Log in as root
$ mysql -u root -p

# Create user
> CREATE USER 'new'@'localhost' IDENTIFIED BY PASSWORD('pw');

# Grant root privileges
> GRANT ALL PRIVILEGES ON * . * TO 'new'@'localhost';

# Change password
> use mysql
> update user set password=PASSWORD("newpw") where User='new';
```

Obviously, sensitive information is withheld. The created user essentially has root privileges. 
