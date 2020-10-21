# SQL基础

## SQL分类
SQL语句分为以下3种：
DDL：数据定义语句，定义了不同的数据段 数据库 表  列 索引等对象，常用关键字create drop alter等

DML：数据操纵语句，用于增加 删除 修改，常用关键字insert，select，update，delete等

DCL：数据控制语句，用于控制不同数据段直接的许可和访问级别的语句。主要关键字包括grant，revoke等。

### DDL语句

1. 创建数据库
启动MySQL服务后，输入以下指令连接到MySQL服务器
```sql
root@VM-0-14-ubuntu:/home/ubuntu# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 550
Server version: 5.7.31-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

mysql表示客户端命令，-u表示连接的数据库用户，-p表示输入的用户密码。
创建数据库：
```sql
mysql> create database test1;
Query OK, 1 row affected (0.00 sec)

```
查看当前mysql服务器中已存在的数据库：
```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| blog               |
| mysql              |
| performance_schema |
| polls              |
| r                  |
| sys                |
| test1              |
+--------------------+
8 rows in set (0.00 sec)
```
其中，information_schema：主要存储了系统中一些数据库对象的信息，比如用户表信息 列信息 权限信息 字符集信息 分区信息等

选择要操作的数据库：
```sql
use dbname;
```
查看数据库中所有的数据表：
```sql
mysql> show tables;
Empty set (0.00 sec)
```
2. 删除数据库
```sql
drop database dbname;
```
示例：
```sql
mysql> drop database test1;
Query OK, 0 rows affected (0.02 sec)
```
注意：数据库删除后，下面所有表数据都会全部删除，所以删除前确保已经完成数据备份

3. 创建表

基本语法：
```sql
create table table_name(
column_name1 column_type1 constraints,
column_name2 column_type2 constraints,
column_name3 column_type3 constraints
);
```
MySQL表名以目录形式存于磁盘，因此表名支持任何目录名允许的字符。
示例：
```sql
mysql> create table emp(ename varchar(30),hiredate date,sal decimal(10,2),deptno int(2));
Query OK, 0 rows affected (0.19 sec)
```
查看表定义：
```sql
mysql> desc emp;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| ename    | varchar(30)   | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(2)        | YES  |     | NULL    |       |
+----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```
查看表定义的sql语句：
```sql
mysql> show create table emp;
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                       |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| emp   | CREATE TABLE `emp` (
  `ename` varchar(30) DEFAULT NULL,
  `hiredate` date DEFAULT NULL,
  `sal` decimal(10,2) DEFAULT NULL,
  `deptno` int(2) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1 |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```
从以上sql语句可以看到，除表定义，还能看到表的engine和charset。

4. 删除表
```sql
drop table table_name ;
```

5. 修改表
大多数情况，表结构的更改都使用alter table 语句。

（1）修改表类型，语法如下：
```sql
alter table table_name modify [column] column_definition [FIRST | after col_name]
```

示例：
```sql
mysql> alter table emp modify ename varchar(2);
Query OK, 0 rows affected (0.25 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc emp;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| ename    | varchar(2)    | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(2)        | YES  |     | NULL    |       |
+----------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

(2)增加表字段，语法如下：
```sql
alter table table_name ADD [column] column_definition [FIRST | after col_name]
```
示例：
```sql
mysql> alter table emp add eid int(11) primary key;       
Query OK, 0 rows affected (0.12 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc emp;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| ename    | varchar(2)    | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(2)        | YES  |     | NULL    |       |
| eid      | int(11)       | NO   | PRI | NULL    |       |
+----------+---------------+------+-----+---------+-------+
5 rows in set (0.01 sec)
```

(3)删除表字段，语法如下：
```sql
alter table table_name drop [column] column_name 
```

(4)字段改名，语法如下：
```sql
alter table table_name CHANGE [column] old_col_name column_definition [FIRST | after col_name]
```
示例
```sql
mysql> alter table emp change ename ename1 varchar(20);
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc emp;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| ename1   | varchar(20)   | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(2)        | YES  |     | NULL    |       |
| eid      | int(11)       | NO   | PRI | NULL    |       |
+----------+---------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```
注意：change与modify都可以修改表定义，但是modify不可以修改列名

(5)修改字段排列顺序
通过前面字段增加和修改语法中的可选项【first|after column_name】
示例：
```sql
mysql> alter table emp modify eid int(11) first;
Query OK, 0 rows affected (0.12 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc
    -> emp;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| eid      | int(11)       | NO   | PRI | NULL    |       |
| ename1   | varchar(20)   | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(2)        | YES  |     | NULL    |       |
+----------+---------------+------+-----+---------+-------+
5 rows in set (0.00 sec)
```

(6)更改表名
```sql
alter table table_name RENAME [to] new_tablename;
```
示例：
```sql
mysql> alter table emp rename employe;
Query OK, 0 rows affected (0.04 sec)

mysql> desc employe;
+----------+---------------+------+-----+---------+-------+
| Field    | Type          | Null | Key | Default | Extra |
+----------+---------------+------+-----+---------+-------+
| eid      | int(11)       | NO   | PRI | NULL    |       |
| ename1   | varchar(20)   | YES  |     | NULL    |       |
| hiredate | date          | YES  |     | NULL    |       |
| sal      | decimal(10,2) | YES  |     | NULL    |       |
| deptno   | int(2)        | YES  |     | NULL    |       |
+----------+---------------+------+-----+---------+-------+
5 rows in set (0.00 sec)

```



### DML语句

 
