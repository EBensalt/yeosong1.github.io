# SQL 문법(feat.mysql)

## 예제로 익히는 SQL 문법

### **mysql 접속**
⌨ `root@35f90b488b4a:/# mysql -u root -p`

~~~
결과

Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 27
Server version: 10.3.22-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

~~~

### **데이터베이스 목록 보기**
⌨ `MariaDB [(none)]> show databases;`

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

### **새 데이터 베이스 생성**
⌨ `MariaDB [(none)]> create database my1;`
⌨ `MariaDB [(none)]> create database my2;`

~~~
결과

Query OK, 1 row affected (0.001 sec)
~~~

### **생성한 DB 확인**
⌨ `MariaDB [(none)]> show databases;`
~~~
결과

+--------------------+
| Database           |
+--------------------+
| information_schema |
| my1                |
| my2                |
| mysql              |
| performance_schema |
+--------------------+
5 rows in set (0.001 sec)
~~~

### **DB 삭제**
⌨ `drop database my2;`

### **방금 만든 my1 DB에 접속하기**
⌨ `MariaDB [(none)]> use my1;`
~~~
결과

Database changed
~~~

### **DB my1에 테이블 생성**
⌨ `MariaDB [my1]> create table my_table_2 (name char(20), price int(20));`
~~~
이렇게 쓸 수도 있다.

    MariaDB [my1]> create table my_table_2 (
    -> name char(20),
    -> price int(20)
    -> );
~~~

### **테이블 구성 보기**
⌨ `MariaDB [my1]> describe my_table_1;` 혹은<br>
⌨ `MariaDB [my1]> desc my_table_1;` 혹은<br>
⌨ `MariaDB [my1]> explain my_table_1;`

~~~
결과

+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| name  | char(20) | YES  |     | NULL    |       |
| price | int(20)  | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
2 rows in set (0.001 sec)
~~~













## mysql --help


#### Usage: mysql [OPTIONS] [database]

mysql 커맨드의 옵션 중 일부

|축약|커맨드|내용|
|:---|:---|:---|
| -u | --user=name  |   User for login if not current user.|
| -p | --password[=name] | |
