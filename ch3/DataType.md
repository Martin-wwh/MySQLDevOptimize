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
