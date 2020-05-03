# SQL 문법(feat.mysql)

## 예제로 익히는 SQL 문법

**mysql 접속** <br>
⌨`root@35f90b488b4a:/# mysql -u root -p`
~~~
결과

Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 27
Server version: 10.3.22-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

~~~

**데이터베이스 보기** <br>
👉MariaDB [(none)]> show databases;
~~~
결과

+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)
~~~

**새 데이터 베이스 생성** <br>
👉`MariaDB [(none)]> create database my1;`
~~~
결과

Query OK, 1 row affected (0.001 sec)
~~~

**생성한 DB 확인** <br>
⌨MariaDB [(none)]> show databases;
~~~
결과

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

**방금 만든 my1 DB에 접속하기** <br>
⌨`MariaDB [(none)]> use my1;`
~~~
결과

Database changed
~~~













## mysql --help
대강 훑어보고, 지금 당장 쓰는 명령어 뜻만 적어둘 것이다.

#### Usage: mysql [OPTIONS] [database]

mysql 커맨드의 옵션

|축약|커맨드|내용|
|:---|:---|:---|
| -u | --user=name  |   User for login if not current user.|
| -p | --password[=name] | |
