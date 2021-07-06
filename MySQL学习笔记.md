# MySQL学习笔记

写在前面：安装和数据库连接在此就不赘述，详情参考其他博客！

**学习参考:[MySQL 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/mysql/mysql-tutorial.html)**

# 1. SQL语句基础语法

```sql
CREATE DATABASE 数据库名; #创建数据库

drop database <数据库名>; #删除数据库

use 数据库名; #选择数据库

show tables; #查看当前库的所有表

create table 表名(   #创建表

	列名 列类型,
	列名 列类型，
	。。。
);

desc 表名; #查看表结构

mysql --version; #查看服务器的版本
```



# 2. SQL语句语法规范

​	1.不区分大小写,但建议关键字大写，表名、列名小写
​	2.每条命令最好用分号结尾
​	3.每条命令根据需要，可以进行缩进 或换行
​	4.注释
​		单行注释：#注释文字
​		单行注释：-- 注释文字
​		多行注释：/* 注释文字  */

### 2.1 SQL语句的分类

- DQL（Data Query Language）：数据查询语言
  		select 

- DML(Data Manipulate Language):数据操作语言
  		insert 、update、delete
- DDL（Data Define Languge）：数据定义语言
  		create、drop、alter
- TCL（Transaction Control Language）：事务控制语言
  		commit、rollback

### 2.2 插入语句

```sql
INSERT INTO table_name ( field1, field2,...fieldN ) VALUES( value1, value2,...valueN ); #插入数据
```

### 2.3 查询语句

```sql
SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]        # 查询数据通用的 SELECT 语法
```

- 查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。
- SELECT 命令可以读取一条或者多条记录。
- 你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据
- 你可以使用 WHERE 语句来包含任何条件。
- 你可以使用 LIMIT 属性来设定返回的记录数。
- 你可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

#### 2.3.1 WHERE 子句

```sql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

- 查询语句中你可以使用一个或者多个表，表之间使用逗号**,** 分割，并使用WHERE语句来设定查询条件。
- 你可以在 WHERE 子句中指定任何条件。
- 你可以使用 AND 或者 OR 指定一个或多个条件。
- WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令。
- WHERE 子句类似于程序语言中的 if 条件，根据 MySQL 表中的字段值来读取指定的数据

#### 2.3.2 LIKE子句

SQL LIKE 子句中使用百分号 **%**字符来表示任意字符，类似于UNIX或正则表达式中的星号 *****。

如果没有使用百分号 **%**, LIKE 子句与等号 **=** 的效果是一样的

```sql
SELECT field1, field2,...fieldN 
FROM table_name
WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'
```

- 可以在 WHERE 子句中指定任何条件。
- 可以在 WHERE 子句中使用LIKE子句。
- 可以使用LIKE子句代替等号 **=**。
- LIKE 通常与 **%** 一同使用，类似于一个元字符的搜索。
- 可以使用 AND 或者 OR 指定一个或多个条件。
- 可以在 DELETE 或 UPDATE 命令中使用 WHERE...LIKE 子句来指定条件。

> ```
> '%a'     //以a结尾的数据
> 'a%'     //以a开头的数据
> '%a%'    //含有a的数据
> '_a_'    //三位且中间字母是a的
> '_a'     //两位且结尾字母是a的
> 'a_'     //两位且开头字母是a的
> ```

**举例**

- 查询以 java 字段开头的信息

```sql
SELECT * FROM position WHERE name LIKE 'java%';
```

- 查询包含 java 字段的信息

```sql
SELECT * FROM position WHERE name LIKE '%java%';
```

- 查询以 java 字段结尾的信息

```sql
SELECT * FROM position WHERE name LIKE '%java';
```

### 2.4 更新语句

```sql
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]
```

- 同时更新一个或多个字段。
- 在 WHERE 子句中指定任何条件。
- 在一个单独表中同时更新数据。

### 2.5 删除语句

```sql
DELETE FROM table_name [WHERE Clause]
```

- 如果没有指定 WHERE 子句，MySQL 表中的所有记录将被删除。
- 可以在 WHERE 子句中指定任何条件
- 可以在单个表中一次性删除记录。

### 2.6 排序语句

SQL SELECT 语句使用 ORDER BY 子句将查询数据排序后再返回数据

```sql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
ORDER BY field1 [ASC [DESC][默认 ASC]], [field2...] [ASC [DESC][默认 ASC]]
```

- 可以使用任何字段来作为排序的条件，从而返回排序后的查询结果。
- 可以设定多个字段来排序。
- 可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。
- 可以添加 WHERE...LIKE 子句来设置条件。

### 2.7 分组语句

GROUP BY 语句根据一个或多个列对结果集进行分组

在分组的列上我们可以使用 COUNT, SUM, AVG,等函数

```sql
SELECT column_name, function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```



# 3. MySQL连接的使用

SELECT, UPDATE 和 DELETE 语句中使用 Mysql 的 JOIN 来联合多表查询

JOIN 按照功能大致分为如下三类：

- **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。
- **LEFT JOIN（左连接）：**获取左表所有记录，即使右表没有对应匹配的记录。
- **RIGHT JOIN（右连接）：** 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录

```sql
select 字段，...
from 表1
【inner|left outer|right outer|cross】join 表2 on  连接条件
【inner|left outer|right outer|cross】join 表3 on  连接条件
【where 筛选条件】
【group by 分组字段】
【having 分组后的筛选条件】
【order by 排序的字段或表达式】
```



# 4. MySQLNULL值处理

MySQL 使用 SQL SELECT 命令及 WHERE 子句来读取数据表中的数据,但是当提供的查询条件字段为 NULL 时，该命令可能就无法正常工作

MySQL提供了三大运算符:

- **IS NULL:** 当列的值是 NULL,此运算符返回 true
- **IS NOT NULL:** 当列的值不为 NULL, 运算符返回 true
- **<=>:** 比较操作符（不同于 = 运算符），当比较的的两个值相等或者都为 NULL 时返回 true

**注意：在语句中 = 和 != 运算符是不起作用的，必须使用  IS NULL 和 IS NOT NULL**



# 5.查询进阶

## 5.1 子查询

一条查询语句中又嵌套了另一条完整的select语句，其中被嵌套的select语句，称为子查询或内查询
在外面的查询语句，称为主查询或外查询

特点

- 1、子查询都放在小括号内
- 2、子查询可以放在from后面、select后面、where后面、having后面，但一般放在条件的右侧
- 3、子查询优先于主查询执行，主查询使用了子查询的执行结果
- 4、子查询根据查询结果的行数不同分为以下两类：
- ① 单行子查询
  	结果集只有一行
  	一般搭配单行操作符使用：> < = <> >= <= 
  	非法使用子查询的情况：
  	a、子查询的结果为一组值
  	b、子查询的结果为空
- ② 多行子查询
  	结果集有多行
  	一般搭配多行操作符使用：any、all、in、not in
  	in： 属于子查询结果中的任意一个就行
  	any和all往往可以用其他查询代替

## 5.2 分页查询

实际的web项目中需要根据用户的需求提交对应的分页查询的sql语句

```sql
select 字段|表达式,...
from 表
【where 条件】
【group by 分组字段】
【having 条件】
【order by 排序的字段】
limit 【起始的条目索引，】条目数;
```

特点

- 1.起始条目索引从0开始
- 2.limit子句放在查询语句的最后
- 3.公式：select * from  表 limit （page-1）*sizePerPage,sizePerPage

> 假如:
> 每页显示条目数sizePerPage
> 要显示的页数 page

## 5.3 联合查询

```sql
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union  【all】
.....
select 字段|常量|表达式|函数 【from 表】 【where 条件】
```

特点

- 1、多条查询语句的查询的列数必须是一致的
- 2、多条查询语句的查询的列的类型几乎相同
- 3、union代表去重，union all代表不去重



# 6. MySQL 正则表达式

| 模式       | 描述                                                         |
| ---------- | :----------------------------------------------------------- |
| ^          | 匹配输入字符串的开始位置                                     |
| $          | 匹配输入字符串的结束位置                                     |
| .          | 匹配除 "\n" 之外的任何单个字符                               |
| [...]      | 字符集合。匹配所包含的任意一个字符                           |
| [^...]     | 负值字符集合。匹配未包含的任意字符                           |
| p1\|p2\|p3 | 匹配 p1 或 p2 或 p3                                          |
| *          | 匹配前面的子表达式零次或多次                                 |
| +          | 匹配前面的子表达式一次或多次                                 |
| {n}        | n 是一个非负整数。匹配确定的 n 次                            |
| {n,m}      | m 和 n 均为非负整数，其中n <= m，最少匹配 n 次且最多匹配 m 次。 |

 举几个例子

- 查找name字段中以'st'为开头的所有数据：

```sql
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^st';
```

- 查找name字段中以'ok'为结尾的所有数据：

```sql
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
```

- 查找name字段中包含'mar'字符串的所有数据：

```sql
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'mar';
```

- 查找name字段中以元音字符开头或以'ok'字符串结尾的所有数据：

```sql
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';
```



# 7. MySQL事务

MySQL 事务主要用于处理操作量大，复杂度高的数据

- 在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。
- 事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行。
- 事务用来管理 insert,update,delete 语句

事务是必须满足4个条件（ACID）：：原子性（**A**tomicity，或称不可分割性）、一致性（**C**onsistency）、隔离性（**I**solation，又称独立性）、持久性（**D**urability）

- **原子性：**一个事务（transaction）中的所有操作，要么全部完成，要么全部不完成，不会结束在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
- **一致性：**在事务开始之前和事务结束以后，数据库的完整性没有被破坏。这表示写入的资料必须完全符合所有的预设规则，这包含资料的精确度、串联性以及后续数据库可以自发性地完成预定的工作。
- **隔离性：**数据库允许多个并发事务同时对其数据进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
- **持久性：**事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

## 7.1 事务控制语句：

- BEGIN 或 START TRANSACTION 显式地开启一个事务；
- COMMIT 也可以使用 COMMIT WORK，不过二者是等价的。COMMIT 会提交事务，并使已对数据库进行的所有修改成为永久性的；
- ROLLBACK 也可以使用 ROLLBACK WORK，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；
- SAVEPOINT identifier，SAVEPOINT 允许在事务中创建一个保存点，一个事务中可以有多个 SAVEPOINT；
- RELEASE SAVEPOINT identifier 删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常；
- ROLLBACK TO identifier 把事务回滚到标记点；
- SET TRANSACTION 用来设置事务的隔离级别。InnoDB 存储引擎提供事务的隔离级别有READ UNCOMMITTED、READ COMMITTED、REPEATABLE READ 和 SERIALIZABLE。

## 7.2 MYSQL 事务处理主要有两种方法：

1、用 BEGIN, ROLLBACK, COMMIT来实现

- **BEGIN** 开始一个事务
- **ROLLBACK** 事务回滚
- **COMMIT** 事务确认

2、直接用 SET 来改变 MySQL 的自动提交模式:

- **SET AUTOCOMMIT=0** 禁止自动提交
- **SET AUTOCOMMIT=1** 开启自动提交

## 7.3 事务的隔离级别

事务并发问题如何发生？

- 当多个事务同时操作同一个数据库的相同数据时

事务的并发问题有哪些？

- 脏读：一个事务读取到了另外一个事务未提交的数据
  不可重复读：同一个事务中，多次读取到的数据不一致
- 幻读：一个事务读取数据时，另外一个事务进行更新，导致第一个事务读取到了没有更新的数据

如何避免事务的并发问题？

> 通过设置事务的隔离级别
> 1、READ UNCOMMITTED
> 2、READ COMMITTED 可以避免脏读
> 3、REPEATABLE READ 可以避免脏读、不可重复读和一部分幻读
> 4、SERIALIZABLE可以避免脏读、不可重复读和幻读

- 设置隔离级别：

```sql
set session|global  transaction isolation level 隔离级别名;
```

- 查看隔离级别:

```sql
select @@tx_isolation;
```



# 8. MySQL索引

MySQL索引的建立对于MySQL的高效运行是很重要的，索引可以大大提高MySQL的检索速度。

索引分单列索引和组合索引。

- 单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。

- 组合索引，即一个索引包含多个列。

**缺点：**

- 虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行INSERT、UPDATE和DELETE。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件。

- 建立索引会占用磁盘空间的索引文件。

## 8.1 普通索引

**创建索引**

```sql
CREATE INDEX indexName ON table_name (column_name)
```

**修改表结构(添加索引)**

```sql
ALTER table tableName ADD INDEX indexName(columnName)
```

**创建表的时候直接指定**

```sql
CREATE TABLE mytable(  
 
ID INT NOT NULL,   
 
username VARCHAR(16) NOT NULL,  
 
INDEX [indexName] (username(length))  
 
);  
```

**删除索引的语法**

```sql
DROP INDEX [indexName] ON mytable; 
```

## 8.2 唯一索引

索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。

**创建索引**

```sql
CREATE UNIQUE INDEX indexName ON mytable(username(length)) 
```

**修改表结构**

```sql
ALTER table mytable ADD UNIQUE [indexName] (username(length))
```

**创建表的时候直接指定**

```sql
CREATE TABLE mytable(  
 
ID INT NOT NULL,   
 
username VARCHAR(16) NOT NULL,  
 
UNIQUE [indexName] (username(length))  
 
);  
```



# 9. MySQL处理重复数据

有些 MySQL 数据表中可能存在重复的记录，有些情况我们允许重复数据的存在，但有时候我们也需要删除这些重复的数据。

**防止表中出现重复数据**

你可以在 MySQL 数据表中设置指定的字段为 **PRIMARY KEY（主键）** 或者 **UNIQUE（唯一）** 索引来保证数据的唯一性。

让我们尝试一个实例：下表中无索引及主键，所以该表允许出现多条重复记录。

```sql
CREATE TABLE person_tbl
(
    first_name CHAR(20),
    last_name CHAR(20),
    sex CHAR(10)
);
```

如果你想设置表中字段 first_name，last_name 数据不能重复，你可以设置双主键模式来设置数据的唯一性， 如果你设置了双主键，那么那个键的默认值不能为 NULL，可设置为 NOT NULL。如下所示：

```sql
CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   PRIMARY KEY (last_name, first_name)
);
```

如果我们设置了唯一索引，那么在插入重复数据时，SQL 语句将无法执行成功,并抛出错。

INSERT IGNORE INTO 与 INSERT INTO 的区别就是 INSERT IGNORE INTO 会忽略数据库中已经存在的数据，如果数据库没有数据，就插入新的数据，如果有数据的话就跳过这条数据。这样就可以保留数据库中已经存在数据，达到在间隙中插入数据的目的。

以下实例使用了 INSERT IGNORE INTO，执行后不会出错，也不会向数据表中插入重复数据：

```sql
mysql> INSERT IGNORE INTO person_tbl (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
Query OK, 1 row affected (0.00 sec)
mysql> INSERT IGNORE INTO person_tbl (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
Query OK, 0 rows affected (0.00 sec)
```

INSERT IGNORE INTO 当插入数据时，在设置了记录的唯一性后，如果插入重复数据，将不返回错误，只以警告形式返回。 而 REPLACE INTO 如果存在 primary 或 unique 相同的记录，则先删除掉。再插入新记录。

另一种设置数据的唯一性方法是添加一个 UNIQUE 索引，如下所示：

```sql
CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   UNIQUE (last_name, first_name)
);
```

**统计重复数据**

以下我们将统计表中 first_name 和 last_name的重复记录数：

```sql
mysql> SELECT COUNT(*) as repetitions, last_name, first_name
    -> FROM person_tbl
    -> GROUP BY last_name, first_name
    -> HAVING repetitions > 1;
```

以上查询语句将返回 person_tbl 表中重复的记录数。 一般情况下，查询重复的值，请执行以下操作：

- 确定哪一列包含的值可能会重复。
- 在列选择列表使用COUNT(*)列出的那些列。
- 在GROUP BY子句中列出的列。
- HAVING子句设置重复数大于1。

**过滤重复数据**

如果你需要读取不重复的数据可以在 SELECT 语句中使用 DISTINCT 关键字来过滤重复数据。

```sql
mysql> SELECT DISTINCT last_name, first_name
    -> FROM person_tbl;
```

你也可以使用 GROUP BY 来读取数据表中不重复的数据：

```sql
mysql> SELECT last_name, first_name
    -> FROM person_tbl
    -> GROUP BY (last_name, first_name);
```

**删除重复数据**

如果你想删除数据表中的重复数据，你可以使用以下的SQL语句：

```sql
mysql> CREATE TABLE tmp SELECT last_name, first_name, sex FROM person_tbl  GROUP BY (last_name, first_name, sex);
mysql> DROP TABLE person_tbl;
mysql> ALTER TABLE tmp RENAME TO person_tbl;
```

当然你也可以在数据表中添加 INDEX（索引） 和 PRIMAY KEY（主键）这种简单的方法来删除表中的重复记录。方法如下：

```sql
mysql> ALTER IGNORE TABLE person_tbl
    -> ADD PRIMARY KEY (last_name, first_name);
```



# 10.MySQL 导出数据

MySQL中你可以使用**SELECT...INTO OUTFILE**语句来简单的导出数据到文本文件上。

**使用 SELECT ... INTO OUTFILE 语句导出数据**

将数据表 runoob_tbl 数据导出到 /tmp/runoob.txt 文件中:

```sql
mysql> SELECT * FROM runoob_tbl 
    -> INTO OUTFILE '/tmp/runoob.txt';
```

你可以通过命令选项来设置数据输出的指定格式，以下实例为导出 CSV 格式：

```sql
mysql> SELECT * FROM passwd INTO OUTFILE '/tmp/runoob.txt'
    -> FIELDS TERMINATED BY ',' ENCLOSED BY '"'
    -> LINES TERMINATED BY '\r\n';
```

**SELECT ... INTO OUTFILE 语句有以下属性:**

- LOAD DATA INFILE是SELECT ... INTO OUTFILE的逆操作，SELECT句法。为了将一个数据库的数据写入一个文件，使用SELECT ... INTO OUTFILE，为了将文件读回数据库，使用LOAD DATA INFILE。
- SELECT...INTO OUTFILE 'file_name'形式的SELECT可以把被选择的行写入一个文件中。该文件被创建到服务器主机上，因此您必须拥有FILE权限，才能使用此语法。
- 输出不能是一个已存在的文件。防止文件数据被篡改。
- 你需要有一个登陆服务器的账号来检索文件。否则 SELECT ... INTO OUTFILE 不会起任何作用。
- 在UNIX中，该文件被创建后是可读的，权限由MySQL服务器所拥有。这意味着，虽然你就可以读取该文件，但可能无法将其删除。

**导出表作为原始数据**

**mysqldump** 是 mysql 用于转存储数据库的实用程序。它主要产生一个 SQL 脚本，其中包含从头重新创建数据库所必需的命令 CREATE TABLE INSERT 等。

**导出 SQL 格式的数据**

导出 SQL 格式的数据到指定文件

```sql
mysqldump -u root -p RUNOOB runoob_tbl > dump.txt
password ******
```

**需要导出整个数据库的数据，可以使用以下命令**

```sql
mysqldump -u root -p RUNOOB > database_dump.txt
password ******
```

**需要备份所有数据库，可以使用以下命令**

```sql
mysqldump -u root -p --all-databases > database_dump.txt
password ******
```



# 11. MySQL导入数据

**1、mysql 命令导入**

```sql
mysql -u用户名    -p密码    <  要导入的数据库数据(xxxx.sql)

mysql -uroot -p123456 < xxxx.sql
```

**2、source 命令导入**

source 命令导入数据库需要先登录到数库终端：

```sql
mysql> create database xxx;      # 创建数据库
mysql> use xxx;                  # 使用已创建的数据库 
mysql> set names utf8;           # 设置编码
mysql> source /xxx/xxx/xxx.sql  # 导入备份数据库
```



# 12.MySQL函数

需要用到来参考

**参考地址：[MySQL 函数 | 菜鸟教程 (runoob.com)](https://www.runoob.com/mysql/mysql-functions.html)**



# 13. MySQL运算符

**参考地址：[MySQL 运算符 | 菜鸟教程 (runoob.com)](https://www.runoob.com/mysql/mysql-operator.html)**

