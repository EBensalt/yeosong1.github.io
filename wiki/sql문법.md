# SQL 문법(feat.mysql)

## mysql --help
대강 훑어보고, 지금 당장 쓰는 명령어 뜻만 적어둘 것이다.

#### Usage: mysql [OPTIONS] [database]
mysql 커맨드의 옵션
|축약|커맨드|내용|
|:---|:---|:---|
| -u | --user=name  |   User for login if not current user.|
| -p | --password[=name] | |


------------------------------------------------------------------------

## 예제로 익히는 SQL 문법

**root@35f90b488b4a:/# mysql -u root -p**
~~~
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 27
Server version: 10.3.22-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

~~~

**MariaDB [(none)]> show databases;**
~~~
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)
~~~

**MariaDB [(none)]> create database my1;**
~~~
Query OK, 1 row affected (0.001 sec)
~~~

**MariaDB [(none)]> show databases;**
~~~
+--------------------+
| Database           |
+--------------------+
| information_schema |
| my1                |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)
~~~

**MariaDB [(none)]> use my1;**
~~~
Database changed
~~~

