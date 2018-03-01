<!-- TOC -->

- [1. MariaDB](#1-mariadb)
    - [1.1. Installation and Configuration](#11-installation-and-configuration)
        - [1.1.1. Install Required Packages](#111-install-required-packages)
        - [1.1.2. Enable IP based connections](#112-enable-ip-based-connections)
        - [1.1.3. Enable service persistently](#113-enable-service-persistently)
        - [1.1.4. Start service](#114-start-service)
        - [1.1.5. Secure installation](#115-secure-installation)
        - [1.1.6. Login to database server (prompt for pw)](#116-login-to-database-server-prompt-for-pw)
        - [1.1.7. Allow access through firewall](#117-allow-access-through-firewall)
        - [1.1.8. Grant access to remote user/ip](#118-grant-access-to-remote-userip)
    - [1.2. Install and Configure Local and Remote Clients](#12-install-and-configure-local-and-remote-clients)
        - [1.2.1. Install MariaDB related tools](#121-install-mariadb-related-tools)
        - [1.2.2. Connect to remote MariaDB server](#122-connect-to-remote-mariadb-server)
        - [1.2.3. Turn off Networking bindings](#123-turn-off-networking-bindings)
    - [1.3. Create a Simple Database Schema](#13-create-a-simple-database-schema)
        - [1.3.1. Connect to Database](#131-connect-to-database)
        - [1.3.2. Viewing Database Information](#132-viewing-database-information)
        - [1.3.3. Create Database](#133-create-database)
        - [1.3.4. Create Table](#134-create-table)
    - [1.4. Perform Simple SQL Queries](#14-perform-simple-sql-queries)
        - [1.4.1. Insert Record](#141-insert-record)
        - [1.4.2. Delete Record](#142-delete-record)
        - [1.4.3. Update Record](#143-update-record)
        - [1.4.4. Selections](#144-selections)
    - [1.5. Backup and Restore Database](#15-backup-and-restore-database)
        - [1.5.1. Dump the Database](#151-dump-the-database)
    - [1.6. Restore the Database](#16-restore-the-database)

<!-- /TOC -->

# 1. MariaDB

## 1.1. Installation and Configuration

### 1.1.1. Install Required Packages

``` shell
# yum groupinstall mariadb mariadb-client
```

### 1.1.2. Enable IP based connections

>*add the following line to /etc/my.cnf*

``` shell
... snip ...

bind-address=<ip-address>

... snip ...
```

### 1.1.3. Enable service persistently

``` shell
# systemctl enable mariadb
```

### 1.1.4. Start service

``` shell
# systemctl start mariadb
```

### 1.1.5. Secure installation

>*set parameters as required during script*

``` shell
# mysql_secure_installation
```

### 1.1.6. Login to database server (prompt for pw)

``` shell
mysql -u root -p
```

### 1.1.7. Allow access through firewall

``` shell
# firewall-cmd --permanent --add-service=mysql
# firewall-cmd --reload
```

### 1.1.8. Grant access to remote user/ip

``` shell
MariaDB [(none)]> GRANT ALL ON *.* TO root@'192.168.33.101' IDENTIFIED BY 'redhat';
FLUSH PRIVILEGES;
```

## 1.2. Install and Configure Local and Remote Clients

### 1.2.1. Install MariaDB related tools

``` shell
# yum groupinstall mariadb mariadb-client
```

### 1.2.2. Connect to remote MariaDB server

``` shell
mysql -h <ip address> -u root -p
```

### 1.2.3. Turn off Networking bindings

>*add the following line to /etc/my.cnf*

``` shell
... snip ...

bind-address=<ip-address>  #Previously Configured
skip-networking=1 #New line

... snip ...
```

## 1.3. Create a Simple Database Schema

### 1.3.1. Connect to Database

``` shell
mysql -u root -p
```

### 1.3.2. Viewing Database Information

``` sql
USE mysql; #go into mysql database
SHOW TABLES; #list tables in mysql
SELECT * FROM host; #return all records from host table
DESCRIBE host; #show field details of host table
```

### 1.3.3. Create Database

``` sql
CREATE DATABASE emails;
```

### 1.3.4. Create Table

``` sql
USE emails;

CREATE TABLE users (userID INT, userFirstName char(25), userLastName char(30), userEmailAddress char(50));
```

## 1.4. Perform Simple SQL Queries

### 1.4.1. Insert Record

``` sql
USE emails;

INSERT INTO users (userID, userFirstName, userLastName, userEmailAddress) VALUES (001, "John", "Doe", "john.doe@gmail.com");
```

### 1.4.2. Delete Record

``` sql
DELETE FROM users WHERE userID = 002;
```

### 1.4.3. Update Record

``` sql
UPDATE users SET userLastName="Jones",userEmailAddress="janejones@example.com" WHERE userID = 3;
```

### 1.4.4. Selections

``` sql
SELECT * FROM users WHERE userID > 4;
SELECT * FROM users ORDER BY userID ASC;
SELECT * FROM users WHERE userFirstName LIKE "%J%";
SELECT * FROM users WHERE userLastName="Smith";
```

## 1.5. Backup and Restore Database

### 1.5.1. Dump the Database

>*Different backup scenarios*

``` shell
mkdir ~/backup
cd ~/backup
mysqldump -u root -p [dbname] > emails_backup.sql
mysqldump -u root -p --databases [db1] [db2] > db1_db2_backup.sql
mysqldump -u root -p --all-databases > all_backup.sql
```

## 1.6. Restore the Database

>*In the case of no database present, create empty database first*

``` sql
CREATE DATABASE emails;
```

>*Then we can restore the backed up database*

``` shell
mysql -u root -p emails < emails_backup.sql
```