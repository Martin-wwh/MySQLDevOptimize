# MySQL支持的数据类型

## 数值类型
mysql支持所有标准sql中的数值类型，包括严格数值类型（INTEGER,SMALLINT,DECIMAL和NUMERIC),以及近似数值类型(FLOAT,REAL和DOUBLE PRECISION).
并基于此，扩展增加了TINYINT,MEDIUMINT和BIGINT这3中不同长度的整形，并增加了BIT类型，用于存放位数据。如下表所示：
|整数类型|字节|最小值|最大值|
|---- |---- |---- |---- |
|TINYINT|1|有符号-128 无符号0|有符号127 无符号255|
|SMALLINT|2|有符号-32768 无符号0|有符号32767 无符号65535|
|MEDIUMINT|3|有符号-pow(2,23) 无符号0|有符号pow(2,23)-1 有符号pow(2,24)-1|
|INT,INTEGER|4|有符号-pow(2,31) 无符号0|有符号pow(2,31)-1 有符号pow(2,32)-1|
|BIGINT|8|有符号-pow(2,63) 无符号0|有符号pow(2,63)-1 有符号pow(2,64)-1|

|浮点数类型|字节|最小值|最大值|
|---- |---- |---- |---- |
|FLOAT |4 | | |
|DOUBLE |8 | | |


|整数类型|字节|最小值|最大值|
|---- |---- |---- |---- |
|DEC(M,D),DECIMAL(M,D) |M+2 |---- |---- |

|位类型|字节|最小值|最大值|
|---- |---- |---- |---- |
|BIT(M) |1-8 |BIT(1) |BIT(64) |
对于整数类型，可以在类型名称后指定显示宽度。一般配合zerofill使用，在数字位数不够时用字符0填满。但是如果插入宽度大于限制宽度的数字时，对插入数据仍然不会有影响，还是按照实际精度保存。

整数还有一个属性：auto_increment.在需要产生唯一标识或顺序值时，可利用此属性，该属性只用于整数。对于使用自增属性的列应该设置为not null。
所有整数类型都有一个可选属性unsigned，该属性加在类型后面。如果一个列指定为zerofill，默认该列添加zerofill属性。

对于小数表示，分为浮点数和定点数。浮点数包括float和double两种，定点数只有decimal。定点数在mysql内部以字符串形式存放，比浮点数更精确，适合用来表示货币等精度高的数据。
浮点数和定点数都可以用类型名称后加"(M,D)"方式进行标识，"(M,D)"表示该值一共显示M位数字（整数+小数位），其中有D位位于小数点后面。
示例，定义float(7,4)的一个列可以显示为-999.9999.MySQL保存值时进行四舍五入。值得注意的是，浮点数后跟"(M,D)"是非标准用法。
```sql
mysql> create table t1(id1 float(5,2) default NULL,id2 double(5,2) default NULL,id3 decimal(5,2) default NULL)      
    -> ;
Query OK, 0 rows affected (0.06 sec)
mysql> insert into t1 values(1.23,1.23,1.23);
Query OK, 1 row affected (0.01 sec)

mysql> insert into t1 values(1.234,1.234,1.23);
Query OK, 1 row affected (0.02 sec)

mysql> select * from  t1;
+------+------+------+
| id1  | id2  | id3  |
+------+------+------+
| 1.23 | 1.23 | 1.23 |
| 1.23 | 1.23 | 1.23 |
+------+------+------+
2 rows in set (0.00 sec)

mysql> insert into t1 values(1.234,1.234,1.234);
Query OK, 1 row affected, 1 warning (0.01 sec)

mysql> select * from  t1;
+------+------+------+
| id1  | id2  | id3  |
+------+------+------+
| 1.23 | 1.23 | 1.23 |
| 1.23 | 1.23 | 1.23 |
| 1.23 | 1.23 | 1.23 |
+------+------+------+
3 rows in set (0.00 sec)

mysql> insert into t1 values(231.23,121.23,561.23);
Query OK, 1 row affected (0.01 sec)

mysql> select * from  t1;
+--------+--------+--------+
| id1    | id2    | id3    |
+--------+--------+--------+
|   1.23 |   1.23 |   1.23 |
|   1.23 |   1.23 |   1.23 |
|   1.23 |   1.23 |   1.23 |
| 231.23 | 121.23 | 561.23 |
+--------+--------+--------+
4 rows in set (0.00 sec)
```
  对于BIT类型，用于存放位字段值，BIT(M)可以用来存放多位二进制数，M范围从1-64，如果不写默认位1.对于位字段，直接用select命令将不会看到结果，可以用bin()或hex()函数读取 。
  示例：
  ```sql
mysql> desc t2;
+-------+--------+------+-----+---------+-------+
| Field | Type   | Null | Key | Default | Extra |
+-------+--------+------+-----+---------+-------+
| id    | bit(1) | YES  |     | NULL    |       |
+-------+--------+------+-----+---------+-------+
1 row in set (0.02 sec)

mysql> insert into t2 values(1);
Query OK, 1 row affected (0.00 sec)

mysql> select * from t2;
+------+
| id   |
+------+
|     |
+------+
1 row in set (0.00 sec)

mysql> select bin(id) from t2;        
+---------+
| bin(id) |
+---------+
| 1       |
+---------+
1 row in set (0.25 sec)
```


## 日期类型
        MySQL中的日期和时间类型
|日期和时间类型|字节|最小值|最大值|
|:----:|:----:|:----:|:----:|
|DATE|4|1000-01-01|9999-12-31|
|DATETIME|8|1000-01-01 00:00:00|9999-12-31 23:59:59|
|TIMESTAMP|4|19700101080001(北京时间)|2038年的某个时刻|
|TIME|3|-838:59:59.000000|838:59:59.000000|
|YEAR|1|1901|2155|
        MySQL中的日期和时间类型的零值表示
|日期和时间类型|零值表示|
|:----:|:----:|
|DATE|0000-00-00|
|DATETIME|0000-00-00 00:00:00|
|TIMESTAMP||
|TIME|00:00:00|
|YEAR|0000|
使用示例：
```sql
mysql> desc t;
+-------+----------+------+-----+---------+-------+
| Field | Type     | Null | Key | Default | Extra |
+-------+----------+------+-----+---------+-------+
| d     | date     | YES  |     | NULL    |       |
| t     | time     | YES  |     | NULL    |       |
| dt    | datetime | YES  |     | NULL    |       |
+-------+----------+------+-----+---------+-------+
3 rows in set (0.00 sec)

mysql> insert into t values(now(),now(),now());
Query OK, 1 row affected, 1 warning (0.00 sec)

mysql> select * from t;
+------------+----------+---------------------+
| d          | t        | dt                  |
+------------+----------+---------------------+
| 2020-10-22 | 20:10:21 | 2020-10-22 20:10:21 |
+------------+----------+---------------------+
1 row in set (0.00 sec)
```
对于timestamp类型，系统自动给表中第一个timestamp的列指定默认值CURRENT_TIMESTAMP(系统日期)。如果有第二个定义为该类型的列，需自行指定默认值。
同时，插入timestamp类型的值时，不能小于该类型的最小值。

timestamp有一个重要特点，和时区相关。当插入日期时，先转换为本地时区后存放；从数据库里面取出时，同样需要将日期转换为本地时区后显示。
这样，两个不同时区的用户看到同一个日期可能是不同的。
查看时区:
```sql
mysql> show variables like 'time_zone';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| time_zone     | SYSTEM |
+---------------+--------+
1 row in set (0.11 sec)
```
时区值为"SYSTEM"，这个值默认与主机的时区值一致。

时区对timestamp值影响的示例：
```sql
mysql> insert into t  values(now(),now());
Query OK, 1 row affected (0.10 sec)

mysql> select * from t;
+---------------------+---------------------+
| id1                 | dt                  |
+---------------------+---------------------+
| 2020-10-26 15:50:36 | 2020-10-26 15:50:36 |
+---------------------+---------------------+
1 row in set (0.00 sec)

mysql> set time_zone='+9:00';
Query OK, 0 rows affected (0.00 sec)

mysql> select * from t;
+---------------------+---------------------+
| id1                 | dt                  |
+---------------------+---------------------+
| 2020-10-26 16:50:36 | 2020-10-26 15:50:36 |
+---------------------+---------------------+
1 row in set (0.00 sec)
```
timestamp与datetime的表示方法很类似，主要区别如下：

1. timestamp支持的时间范围比较小，取值范围从19700101080001到2038年的某个时间，而datetime从1000-01-01 00:00:00到9999-12-31 23:59:59。
2. 表中第一个timestamp列自动设置为系统时间。如果在一个timestamp列中插入null，该列值自动设置为当前日期和时间。在插入或更新一行但不明确指定timestamp列值也会自动设置该列的值为当前的日期和时间。
3. timestamp插入和查询受时区影响，更能反映出实际的时间。而datetime只能反映插入时当地时区表示的时间，其他时区的用户看到的时间会有误差。
4. timestamp的属性受到mysql版本和服务器SQLMode的影响很大。

什么样格式的数据可以正确插入到对应的日期字段中？
以datetime为例：
1. yyyy-mm-dd HH:MM:SS或yy-mm-dd HH:MM:SS格式的字符串。允许不严格语法，，即任何标点符号都可以作为日期部分或事件部分之间的分隔符。
```sql
mysql> insert into t values(null, "2020/10/11 12+12+12");
Query OK, 1 row affected (0.01 sec)

mysql> select * from t;
+---------------------+---------------------+
| id1                 | dt                  |
+---------------------+---------------------+
| 2020-10-26 15:50:36 | 2020-10-26 15:50:36 |
| 2020-10-26 15:57:55 | 2020-10-26 15:57:55 |
| 2020-10-26 16:21:31 | NULL                |
| 2020-10-26 16:25:18 | 2020-10-11 12:12:12 |
+---------------------+---------------------+
4 rows in set (0.00 sec)
```
2. yyyymmddHHMMSS或yymmddHHMMSS格式的无间隔符的字符串。
3. yyyymmddHHMMSS或yymmddHHMMSS格式的数字，假定数字对于日期类型是有意义的。
4. 函数返回的结果，其值适合datetime date或timestamp上下文，如NOW()或CURRENT_DATE。


## 字符串类型
        MySQL中的字符类型
|字符串类型|字节|描述及存储需求|
|:----|:----|:----|
|CHAR(M)|M|M为0-255之间的整数|
|VARCHAR(M)|  |M为0-65535之间的整数，值得长度+1个字节|
|TINYBLOB|  |允许长度0-255字节，值的长度+1个字节|
|BLOB|  |允许长度0-65535字节，值的长度+2个字节|
|MEDIUMBLOB|  |允许长度0-167772150字节，值的长度+3个字节|
|LONGBLOB|  |允许长度0-4294967295字节，值的长度+4个字节|
|TINYTEXT|  |允许长度0-255字节，值的长度+2个字节|
|TEXT|  |允许长度0-655335字节，值的长度+2个字节|
|MEDIUMTEXT|  |允许长度0-167772150字节，值的长度+3个字节|
|LONGTEXT|  |允许长度0-4294967295字节，值的长度+4个字节|
|VARBINARY(M)|  |允许长度0-M字节的变长字节字符串，值的长度+1个字节|
|BINARY(M)|  |允许长度0-M字节的定长字节字符串|

### CHAR与VARCHAR类型
两者类似，都用于存储较短的字符串。区别在于存储方式不同：CHAR列的长度固定为创建时声明的长度，而varchar列中的值为可变长字符串 。在检索时，char列删除了尾部的空格，而varchar列保留这些空格。
示例:
```sql
mysql> create table vc(v varchar(4),c char(4));
Query OK, 0 rows affected (0.03 sec)

mysql> insert into vc values('ab  ','ab  ');
Query OK, 1 row affected (0.01 sec)

mysql> select length(v),length(c) from vc;
+-----------+-----------+
| length(v) | length(c) |
+-----------+-----------+
|         4 |         2 |
+-----------+-----------+
1 row in set (0.00 sec)
```

### BINARY和VARBINARY类型
与上面两种类似，不同的是它们包含二进制字符串而不包含非二进制字符串。示例：
```sql
mysql> create table t(c binary(3));
Query OK, 0 rows affected (0.03 sec)

mysql> insert into t set  c='a';
Query OK, 1 row affected (0.01 sec)

mysql> select *,hex(c),c='a',c='a\0',c='a\0\0' from t;
+------+--------+-------+---------+-----------+
| c    | hex(c) | c='a' | c='a\0' | c='a\0\0' |
+------+--------+-------+---------+-----------+
| a    | 610000 |     0 |       0 |         1 |
+------+--------+-------+---------+-----------+
1 row in set (0.04 sec)
```

### ENUM类型
枚举类型，其取值范围在创建表时通过枚举方式显式指定，对1-255个成员的枚举需要1个字节存储；对于255-65535个成员，需要2个字节存储。
示例：
```sql
CREATE table t (gender enum('M','W'));
insert into t values('M'),('1'),('m'),(NULL);
```
enum类型是忽略大小写的，插入数字n，分别表示枚举取值范围内的第n个值。n不允许超出枚举数量的最大值。

### set类型
与枚举类型很类似，也是一个字符串对象，里面可以包含0-64个成员。


