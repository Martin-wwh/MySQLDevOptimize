# 常用函数
在mysql数据库中，函数可以用在select update delete语句及其子句中。

## 字符串函数
常用函数：

|函数|功能|
|:---|:---|
|CONCAT(s1,s2,...,sN)|连接s1,...,sN为一个字符串|
|INSERT(str,x,y,instr)|将字符串str从第x个位置开始，y个字符长的子串替换为字符串 instr|
|LOWER(str)| |
|UPPER(str)| |
|LEFT(str,x)| |
|RIGHT(str,x)| |
|LPAD(str,n,pad)| 用字符串pad对str最左边进行填充，直到长度为n个字符长度 |
|RPAD(str,n,pad)| 用字符串pad对str最左边进行填充，直到长度为n个字符长度 |
|LTRIM(str)| 去掉字符串str左侧的空格 |
|RTRIM(str)| 去掉字符串str行尾的空格 |
|REPEAT(str,x)| 返回str重复x次的结果 |
|REPLACE(str,a,b)| 用字符串b替换替换str中所有出现的字符串a |
|TRIM(str)| 去掉字符串行尾和行头的空格 |
|SUBSTRING(str,x,y)| 返回从字符串str x位置起y个字符串长度的子串 |


## 数值函数 
MySQL中常用的数值函数

|函数|功能|
|:---|:---|
|ABS(X)|取X的绝对值|
|CEIL(X)|对X向上取整|
|FLOOR(X)|对X向下取整|
|MOD(X,Y)|取X/Y的模|
|RAND()|返回0-1的随机数|
|ROUND(X,Y)|返回X的四舍五入的有Y位小数的值|
|TRUNCATE(X,Y)|返回数字X截断为Y位小数的结果|

## 日期和时间函数
MySQL中常用的日期和时间函数

|函数|功能|
|:---|:---|
|CURDATE() |返回当前日期 |
|CURTIME() |返回当前时间 |
|NOW() |返回当前的日期和时间 |
|UNIX_TIMESTAMP(date) |返回日期date的UNIX时间戳  |
|FROM_UNIXTIME(unixtime) |返回UNIX时间戳的日期值 |
|WEEK(date) |返回日期date是一年中的第几周 |
|YEAR(date) |返回日期date的年份值 |
|HOUR(time) |返回时间time的小时值 |
|MINUTE(time) |返回时间time的分钟值 |
|MONTHNAME(date) |返回日期date的月份名 |
|DATE_FORMAT(date,fmt) |返回按字符串fmt格式化的日期date |
|DATE_ADD(date,INTERVAL expr type) |返回一个日期或时间值加上一个时间间隔的时间值 |
|DATEDIFF(expr,expr2) |返回起始时间expr与结束时间expr2之间的天数 |

## 流程函数
MySQL中的流程函数

|函数|功能|
|:---|:---|
|IF(value,t , f)|如果value为真，返回t；否则返回f|
|IFNULL(value1,value2)|如果value1不为空，返回value1，否则返回value2|
|CASE WHEN [value1] THEN[result1]...ELSE [default] end|如果value1为真，返回result1，否则返回default|
|CASE [expr] WHEN [value1] THEN[result1]...ELSE [default] end|如果expr等于value1，返回result1，否则返回default|


## 其他常用函数

|函数|功能|
|:---|:---|
|DATABASE()| |
|VERSION()| |
|USER()| |
|INET_ATON(IP)| |
|INET_NTOA(num)| |
|PASSWORD(str)| |
|MD5()| |







