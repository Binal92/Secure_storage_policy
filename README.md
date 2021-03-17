# Secure_storage_policy
System requirements (Used environment)

Virtual Machine with below specs
OS - Kali 5.9.0
CPU - 1
Memory - 4GB
Disk - 40 GB

Packages installation

Apache2
Php
Mariadb

Database setup 

For data encryption
mysqld --plugin-load file_key_management
mysqld --plugin-load-add file_key_management

Key file setup 
mkdir  /etc/mysql/encryption/keyfile
openssl rand -hex 32 >> /etc/mysql/encryption/keyfile

Edit generated key as below.
100;8db1ee74580e7e93ab8cf157f02656d356c2f437d548d5bf16bf2a56932954a3

Add keyfile and encryption algorithm
mysqld --loose_file_key_management_filename  /etc/mysql/encryption/keyfile
mysqld --loose_file_key_management_encryption_algorithm  AES_CTR

Edit below file as well
vim /etc/mysql/conf.d/mariadb.cnf
[mariadb]
plugin_load_add = file_key_management
loose_file_key_management_filename = /etc/mysql/encryption/keyfile
innodb_encrypt_log = ON

Enable encryption on tables spaces
SET GLOBAL innodb_encryption_threads=4;
SET GLOBAL innodb_encrypt_tables=ON;
SET SESSION innodb_default_encryption_key_id=100;


Create Primary DB
create database emp_db;

CREATE TABLE emp_db.employee (id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY, name varchar(255) NOT NULL, skills varchar(255) NOT NULL, address varchar(255) NOT NULL, designation varchar(255) NOT NULL, age int(11) NOT NULL, deleted boolean DEFAULT 0, UNIQUE (id)) ENGINE=InnoDB DEFAULT CHARSET=latin1;


Create Secondary DB
create database emp_db_bak;

CREATE TABLE emp_db_bak.employee (id INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY, name varchar(255) NOT NULL, skills varchar(255) NOT NULL, address varchar(255) NOT NULL, designation varchar(255) NOT NULL, age int(11) NOT NULL, deleted boolean DEFAULT 0, UNIQUE (id)) ENGINE=InnoDB DEFAULT CHARSET=latin1;

Web server setup
Unzip  the db_storage folder in /var/www/html/ folder
(Can get from github as well git clone https://github.com/Binal92/Secure_storage_policy )
Change the database parameters in config.php
Restart apache2 server
