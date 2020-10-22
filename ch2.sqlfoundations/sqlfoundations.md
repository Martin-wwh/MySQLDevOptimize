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
**注意：数据库删除后，下面所有表数据都会全部删除，所以删除前确保已经完成数据备份**

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
**注意：change与modify都可以修改表定义，但是modify不可以修改列名**

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
对数据库中表记录的操作，主要包括表记录的插入(insert) 更新(update) 删除(delete)和查询(select)
 
 1. 插入记录
 ```sql
insert into table_name(field1, field2,...,fieldn)  values(v1,v2,v3,...,v3)
```
示例：
```sql
mysql> insert into employe(ename1,hiredate,sal,deptno) values("marin",'2020-10-01','2000',1);
Query OK, 1 row affected (0.03 sec)
mysql> insert into employe values(2,"kangkang",'2020-01-01','3000',2);
Query OK, 1 row affected (0.00 sec)

mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 3000.00 |      2 |
+-----+----------+------------+---------+--------+
2 rows in set (0.00 sec)
```
含可空字段 非空但是有默认值的字段 自增字段，可以不用在insert后的字段列表中出现，values后只写对应的字段value
。

insert语句支持一次插入多条记录，语法如下：
```sql
insert into table_name(field1, field2,...,fieldn)  values(record1_v1,record1_v2,record1_v3,...,record1_v3),
(record2_v1,record2_v2,record1_v3,...,record2_v3),
(record3_v1,record3_v2,record3_v3,...,record3_v3);
```
示例：
```sql
mysql> insert  into employe(ename1, hiredate) values("hong",now()),("king",now()),("qian",now());
Query OK, 3 rows affected, 3 warnings (0.02 sec)
Records: 3  Duplicates: 0  Warnings: 3

mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 3000.00 |      2 |
|   3 | hong     | 2020-10-21 |    NULL |   NULL |
|   4 | king     | 2020-10-21 |    NULL |   NULL |
|   5 | qian     | 2020-10-21 |    NULL |   NULL |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)
```

2. 更新记录
update命令，语法如下：
```sql
update table_name set field1=v1,field2=v2,...,fieldn=vn  [where condition]
```

在MySQL中，update命令支持同时更新多个表中数据，语法如下：
```sql
update t1,t2,...,tn set t1.field1=expr1,tn.fieldn=exprn  [where condition]
```
示例：
```sql
mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 3000.00 |      2 |
|   3 | hong     | 2020-10-21 |    NULL |   NULL |
|   4 | king     | 2020-10-21 |    NULL |   NULL |
|   5 | qian     | 2020-10-21 |    NULL |   NULL |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)

mysql> select * from dept;
+--------+----------+
| deptno | deptname |
+--------+----------+
|      1 | tech     |
|      2 | sale     |
|      3 | fin      |
+--------+----------+
3 rows in set (0.00 sec)

mysql> update employe a, dept b set a.sal=a.sal*b.deptno,b.deptname=a.ename1 where a.deptno=b.deptno;
Query OK, 3 rows affected (0.00 sec)
Rows matched: 4  Changed: 3  Warnings: 0

mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 6000.00 |      2 |
|   3 | hong     | 2020-10-21 |    NULL |   NULL |
|   4 | king     | 2020-10-21 |    NULL |   NULL |
|   5 | qian     | 2020-10-21 |    NULL |   NULL |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)

mysql> select * from dept;
+--------+----------+
| deptno | deptname |
+--------+----------+
|      1 | marin    |
|      2 | kangkang |
|      3 | fin      |
+--------+----------+
3 rows in set (0.00 sec)

```
**注意：多表更新的语法更多用在根据一个表的字段来动态更新另一个表的字段**

3. 删除记录
用delete命令删除记录，语法如下：
```sql
delete from table_name [where condition];
```

4. 查询记录
用select可以执行各种各样的查询 ，最基本的语法：
```sql
select * from table_name [where condition]
```
(1)查询不重复的记录
```sql
mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 6000.00 |      2 |
|   3 | hong     | 2020-10-21 |    NULL |      3 |
|   4 | king     | 2020-10-21 |    NULL |      1 |
|   5 | qian     | 2020-10-21 |    NULL |      1 |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)

mysql> select distinct deptno from employe;
+--------+
| deptno |
+--------+
|      1 |
|      2 |
|      3 |
+--------+
3 rows in set (0.00 sec)
```
(2)条件查询

用where关键字实现根据特定条件查询一部分数据,示例：
```sql
mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 6000.00 |      2 |
|   3 | hong     | 2020-10-21 |    NULL |      3 |
|   4 | king     | 2020-10-21 |    NULL |      1 |
|   5 | qian     | 2020-10-21 |    NULL |      1 |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)

mysql> select * from employe where deptno=1;
+-----+--------+------------+---------+--------+
| eid | ename1 | hiredate   | sal     | deptno |
+-----+--------+------------+---------+--------+
|   1 | marin  | 2020-10-01 | 2000.00 |      1 |
|   4 | king   | 2020-10-21 |    NULL |      1 |
|   5 | qian   | 2020-10-21 |    NULL |      1 |
+-----+--------+------------+---------+--------+
3 rows in set (0.00 sec)
```
where后面的条件，还可以使用> < >= <= !=等比较运算符；多个条件之间还可以用or and等逻辑运算符进行多条件联合查询
示例：
```sql
mysql> select * from employe;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 2000.00 |      2 |
|   3 | hong     | 2020-10-21 | 4000.00 |      3 |
|   4 | king     | 2020-10-21 | 5000.00 |      1 |
|   5 | qian     | 2020-10-21 | 2000.00 |      1 |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)

mysql> select * from employe where deptno=1 and sal<3000;
+-----+--------+------------+---------+--------+
| eid | ename1 | hiredate   | sal     | deptno |
+-----+--------+------------+---------+--------+
|   1 | marin  | 2020-10-01 | 2000.00 |      1 |
|   5 | qian   | 2020-10-21 | 2000.00 |      1 |
+-----+--------+------------+---------+--------+
2 rows in set (0.00 sec)
```
(3) 排序和限制

排序通过关键字order by实现，语法如下：
```sql
select * from table_name [where condition] [order by field1 [DESC|ASC ],field2 [DESC|ASC],...,fieldn[DESC|ASC]]
```
其中DESC与ASC是排序顺序关键字，DESC表示按字段降序排列，ASC反之，如果不写，则默认为ASC。
示例：
```sql
mysql> select * from employe order by sal  ASC, eid ASC;
+-----+----------+------------+---------+--------+
| eid | ename1   | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 2000.00 |      2 |
|   5 | qian     | 2020-10-21 | 2000.00 |      1 |
|   3 | hong     | 2020-10-21 | 4000.00 |      3 |
|   4 | king     | 2020-10-21 | 5000.00 |      1 |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)
```
如果排序字段值一样，则值相同的字段按照第二个字段排序，以此类推。如果只有一个排序字段，则字段相同的记录将无序排列。

对于排序后的字段，如果只希望显示一部分而不是全部显示，可以使用limit关键字，语法如下：
```sql
select ... [LIMIT offset_start,row_count]
```
offset_start表示记录的起始偏移量，row_count表示显示的行数。默认情况下，起始偏移量是0，只需要写记录行数就行。limit经常order by一起配合用于分页显示。

**注意：limit属于MySQL扩展SQL92后的语法，在其他数据库不能通用。**

(4) 聚合

很多时候用户需要进行一些汇总操作，如统计整个公司人数或每个部门人数等，这是需要 用到sql的聚合操作。语法如下：
```sql
select [field1,field2,...,fieldn] fun_name from table_name [where condition] [GROUP BY field1,field2,...,fieldn with ROLLUP] [HAVING where_condition] 
```
fun_name是表示要进行的聚合操作,即聚合函数，常用的有sum count max min等

group by关键字表示要进行分类聚合的字段，比如按照部门分类统计员工数量，部门应该写在group by之后。

with rollup是可选语法，表示是否对分类聚合后的结果再进行汇总。

having关键字表示对分类后的结果再进行条件的过滤。

**注意：having和where的区别在于，having是对聚合后的结果进行条件的过滤，而where是在聚合前就对记录进行过滤，如果逻辑允许，尽可能用where先过滤，因为这样结果集减小，对聚合的效率大大提高，最后再根据逻辑看是否用having进行过滤。**

示例，在employe表中统计公司总人数，并在此基础上统计各部门人数：
```sql
mysql> select count(*) from employe;
+----------+
| count(*) |
+----------+
|        5 |
+----------+
1 row in set (0.00 sec)

mysql> select deptno, count(*) from employe group by deptno;
+--------+----------+
| deptno | count(*) |
+--------+----------+
|      1 |        3 |
|      2 |        1 |
|      3 |        1 |
+--------+----------+
3 rows in set (0.00 sec)
```
既要统计各部门人数，还要统计总人数：
```sql
mysql> select deptno, count(*) from employe group by deptno with rollup;
+--------+----------+
| deptno | count(*) |
+--------+----------+
|      1 |        3 |
|      2 |        1 |
|      3 |        1 |
|   NULL |        5 |
+--------+----------+
4 rows in set (0.00 sec)
```
统计人数大于1人的部门：
```sql
mysql> select deptno, count(*) from employe group by deptno having count(*)>1;
+--------+----------+
| deptno | count(*) |
+--------+----------+
|      1 |        3 |
+--------+----------+
1 row in set (0.00 sec)
```
最后统计公司所有员工的薪水总额 最高和最低薪水：
```sql
mysql> select count(sal),max(sal),min(sal) from employe;
+------------+----------+----------+
| count(sal) | max(sal) | min(sal) |
+------------+----------+----------+
|          5 |  5000.00 |  2000.00 |
+------------+----------+----------+
1 row in set (0.00 sec)

```
(5)表连接
需要同时显示多个表中的字段，就可以用表连接来实现。从大类分，表连接分为外连接与内连接，他们之间最主要的区别是，内连接仅选出两张表中相互匹配的记录，而外连接会选出其他不匹配的记录。
最常用的是内连接。
例如，查出所有雇员名字与所在部门名字，由于雇员名字与部门名字两个字段分别存于表employe与dept中，所以需要用到表连接：
```sql
mysql> select * from dept;
+--------+----------+
| deptno | deptname |
+--------+----------+
|      1 | tech     |
|      2 | sale     |
|      3 | hr       |
+--------+----------+
3 rows in set (0.00 sec)

mysql> select ename,deptname from employe,dept where employe.deptno=dept.deptno;
+----------+----------+
| ename    | deptname |
+----------+----------+
| marin    | tech     |
| kangkang | sale     |
| hong     | hr       |
| king     | tech     |
| qian     | tech     |
+----------+----------+
5 rows in set (0.00 sec)
```
外连接分为左连接与右连接，具体定义如下：
左连接：包含所有的左边表的记录甚至是右边表中没有和它相匹配的记录。(left join)
右连接：包含所有的右边表的记录甚至是左边表中没有和它相匹配的记录。(right join)
左连接与右连接可以相互转化。

(6)子查询

某些情况下，进行查询时，需要的条件是另一个select语句的结果，这时候就要用到子查询。用于子查询的关键字主要包括in、not in、=、!=、exists、not exists等
示例，从employe表中查询出所有部门在dept表中的所有记录：
```sql
mysql> select * from employe;
+-----+-----------+------------+----------+--------+
| eid | ename     | hiredate   | sal      | deptno |
+-----+-----------+------------+----------+--------+
|   1 | marin     | 2020-10-01 |  2000.00 |      1 |
|   2 | kangkang  | 2020-01-01 |  2000.00 |      2 |
|   3 | hong      | 2020-10-21 |  4000.00 |      3 |
|   4 | king      | 2020-10-21 |  5000.00 |      1 |
|   5 | qian      | 2020-10-21 |  2000.00 |      1 |
|   6 | xiaozhang | 2020-10-22 | 16000.00 |      4 |
|   7 | xiaopan   | 2020-10-22 | 13000.00 |      4 |
+-----+-----------+------------+----------+--------+
7 rows in set (0.00 sec)

mysql> select * from dept;
+--------+----------+
| deptno | deptname |
+--------+----------+
|      1 | tech     |
|      2 | sale     |
|      3 | hr       |
|      5 | fin      |
+--------+----------+
4 rows in set (0.00 sec)

mysql> select * from employe where deptno in (select deptno from dept);
+-----+----------+------------+---------+--------+
| eid | ename    | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 2000.00 |      2 |
|   3 | hong     | 2020-10-21 | 4000.00 |      3 |
|   4 | king     | 2020-10-21 | 5000.00 |      1 |
|   5 | qian     | 2020-10-21 | 2000.00 |      1 |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)
```
如果子查询记录数是唯一的，还可以用=代替in。

某些情况下子查询可以转化为表连接。示例：
```sql
mysql> select * from employe where deptno in (select deptno from dept);
+-----+----------+------------+---------+--------+
| eid | ename    | hiredate   | sal     | deptno |
+-----+----------+------------+---------+--------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |
|   2 | kangkang | 2020-01-01 | 2000.00 |      2 |
|   3 | hong     | 2020-10-21 | 4000.00 |      3 |
|   4 | king     | 2020-10-21 | 5000.00 |      1 |
|   5 | qian     | 2020-10-21 | 2000.00 |      1 |
+-----+----------+------------+---------+--------+
5 rows in set (0.00 sec)

mysql> select * from employe inner join dept on employe.deptno=dept.deptno;
+-----+----------+------------+---------+--------+--------+----------+
| eid | ename    | hiredate   | sal     | deptno | deptno | deptname |
+-----+----------+------------+---------+--------+--------+----------+
|   1 | marin    | 2020-10-01 | 2000.00 |      1 |      1 | tech     |
|   2 | kangkang | 2020-01-01 | 2000.00 |      2 |      2 | sale     |
|   3 | hong     | 2020-10-21 | 4000.00 |      3 |      3 | hr       |
|   4 | king     | 2020-10-21 | 5000.00 |      1 |      1 | tech     |
|   5 | qian     | 2020-10-21 | 2000.00 |      1 |      1 | tech     |
+-----+----------+------------+---------+--------+--------+----------+
5 rows in set (0.00 sec)
```
**注意：表连接在很多情况下用于优化子查询。**

(7) 记录联合

将两个表的数据按照一定的查询条件查出来后，将结果合并到一起显示出来，这时候，需要用到union和union all关键字实现，具体语法如下：
```sql
select * from  t1 union|union all select * from t2 ... union|union all select * from tn;
```
union与union all的区别在于union是将union all的结果进行一次distinct，去除重复记录后的结果。
看如下实例：
```sql
mysql> select deptno from employe union all select deptno from dept;  
+--------+
| deptno |
+--------+
|      1 |
|      2 |
|      3 |
|      1 |
|      1 |
|      4 |
|      4 |
|      1 |
|      2 |
|      3 |
|      5 |
+--------+
11 rows in set (0.00 sec)

mysql> select deptno from employe union select deptno from dept;    
+--------+
| deptno |
+--------+
|      1 |
|      2 |
|      3 |
|      4 |
|      5 |
+--------+
5 rows in set (0.00 sec)
```
**注意:UNION 内部的 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的列的顺序必须相同。**

示例：
```sql
mysql> select deptno,ename from employe union select deptno,deptname from dept;
+--------+-----------+
| deptno | ename     |
+--------+-----------+
|      1 | marin     |
|      2 | kangkang  |
|      3 | hong      |
|      1 | king      |
|      1 | qian      |
|      4 | xiaozhang |
|      4 | xiaopan   |
|      1 | tech      |
|      2 | sale      |
|      3 | hr        |
|      5 | fin       |
+--------+-----------+
11 rows in set (0.00 sec)
```

