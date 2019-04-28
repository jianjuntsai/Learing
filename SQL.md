# SQL学习笔记

## 1.表设计
### 1.1 三范式
- 第一范式
    - 数据具有原子性（把数据分割成创建有效率的表所需的最小片段）
        - 具有原子性的数据列中不会有多个类型相同的值（人的兴趣）
        - 具有原子性数据的表中不会有多个存储同类数据的列（兴趣1、兴趣2）
    - 每个数据行必须有独一无二的识别项，即主键（Primary Key）
        - 主键是一个列，它可以让每一条记录成为唯一的
- 第二范式
	- 当某例的数据必须随着另一列的数据改变而改变时，表示第一列函数依赖于第二列
	- 部分函数依赖，是指非主键的列依赖于组合主键的某个部分，但不是完全依赖于组合主键
	- 第二范式：没有部分函数依赖
- 第三范式
	- 传递函数依赖，如果改变任何非常键列可能造成其他列的改变，即为传递依赖，即任何非主键列与另一个非主键列有关联
	- 第三范式：没有传递函数依赖性

## 2.单表操作
### 2.1 CREAT & DESC

#### CREAT TABLE
To create a new table in SQLite, you use CREATE TABLE statement using the following syntax:

```
CREATE TABLE [IF NOT EXISTS] [schema_name].table_name (
 column_1 data_type PRIMARY KEY AUTOINCREMENT,
   column_2 data_type NOT NULL,
 column_3 data_type DEFAULT 0,
column_4 data_type UNIQUE,
column_5 data_type CHECK,
PRIMARY KEY(<filedname1,filedname2>)
 table_constraint
) [WITHOUT ROWID];
```

#### 显示已建表格的结构
```
SHOW CREATE DATABASE <datebasename>;
SHOW CREATE TABLE <table name>;
SHOW COLUMNS FROM <tablename>;
```
#### Data Types
Storage Class | Meaning
---|---
NULL| NULL values mean missing information or unknown.
INTEGER | Integer values are whole numbers (either positive or negative). An integer can have variable sizes such as 1, 2,3, 4, or 8 bytes.
REAL | Real values are real numbers with decimal values that use 8-byte floats.
TEXT |TEXT is used to store character data. The maximum length of TEXT is unlimited. SQLite supports various character encodings.
BLOB | BLOB stands for a binary large object that can be used to store any kind of data. The maximum size of BLOBs is unlimited.

显示数据类型

```
SECECT typeof(字段)
```


#### Foreign Key

外键(foreign key)是表中的某一列，它引用到另一个表的主键(primary key)
外键使用的主键被称为父键（parent key）
父键所在的表被称为父表(parent table)

也可以使用外键来引用父表中的某个唯一的值，外键不一定必须是父表的主键，但是外键在父表必须具有唯一性

在创建表时，创建外键。
To add the foreign key constraint to the suppliers table, you change the definition of the CREATE TABLE statement above as follows:

```
DROP TABLE IF EXISTS suppliers;
 
CREATE TABLE suppliers (
 supplier_id integer PRIMARY KEY,
 supplier_name text NOT NULL,
 group_id integer NOT NULL,
        FOREIGN KEY (group_id) REFERENCES supplier_groups(group_id)
);
```

To specify how foreign key constraint behaves whenever the parent key is deleted or updated, you use the ON DELETE or ON UPDATE action as follows:


```
FOREIGN KEY (foreign_key_columns)
 REFERENCES parent_table(parent_key_columns)
 ON UPDATE action 
 ON DELETE action;
```
SQLite provides the following actions: SET NULL, SET DEFAULT, RESTRICT, CASCADE, and NO ACTION.

SET NULL

When the parent key changes, delete or update, the corresponding child keys of all rows set to a NULL value.
First, recreate the suppliers table using the SET NULL action:

```
DROP TABLE suppliers;
 
CREATE TABLE IF EXISTS suppliers (
 supplier_id integer PRIMARY KEY,
 supplier_name text NOT NULL,
 group_id integer,
 FOREIGN KEY (group_id) REFERENCES supplier_groups (group_id) 
  ON UPDATE SET NULL
);
```


#### 根据查询结果创建表
```
CREATE TABLE <table name> AS
  SELECT <column name> FROM <table name>
  GROUP BY
  ORDER BY
```




#### DESC
```
DESC <tables>
```

### 2.2 INSERT INTO
```
INSERT INTO table_name (column_name1,column_name2,...)
VALUES
('value1','values2',,...)
```

insert Inquiry
```
INSERT INTO <table name> (column_name1,column_name2,...)
SELECT (column_name1,column_name2,...) FROM <table name>
GROUP BY
ORDER BY
```


### 2.3 UPDATE
```
UPDATE table
SET column_1 = new_value_1,
    column_2 = new_value_2
WHERE
    search_condition 
ORDER column_or_expression
LIMIT row_count OFFSET offset;
```

### 2.4 DROP TABLE

```
DROP TABLE <table name>
```

### 2.5 DELETE
 The syntax of the SQLite DELETE statement is as follows:
```
DELETE
FROM 
   table
WHERE search_condition
ORDER BY criteria
LIMIT row_count OFFSET offset;
```

### 2.6 CASE循环

```
CASE case_expression
     WHEN when_expression_1 THEN result_1
     WHEN when_expression_2 THEN result_2
     ...
     [ ELSE result_else ] 
END
```
example

```
SELECT
 customerid,
 firstname,
 lastname,
 CASE country
 WHEN 'USA' THEN
 'Dosmetic'
 ELSE
 'Foreign'
 END CustomerGroup
FROM
 customers
ORDER BY
 LastName,
 FirstName;
```

```
CASE
     WHEN bool_expression_1 THEN result_1
     WHEN bool_expression_2 THEN result_2
     [ ELSE result_else ] 
END
```
example

```
SELECT
 trackid,
 name,
 CASE
 WHEN milliseconds < 60000 THEN
 'short'
 WHEN milliseconds > 6000 AND milliseconds < 300000 THEN 'medium'
 ELSE
 'long'
 END category
FROM
 tracks;
```


### 2.7 ALTER


#### ADD 添加链，并修改主键

```
ALTER TABLE <tablename>
ADD COLUMN <columnname> INT NOT NULL AUTOINCREMENT FIRST/AFTER  <columnname>/,AFTER  <columnname>
ADD PRIMARY KEY(columnname)
```

### CHANGE 修改列名和数据类型
```
ALTER TABLE <table name>
CHANGE COLUMN <field name> <new field name> INT NOT NULL AUTOINCREMENT,
ADD PRIMARY KEY (field name)
```

### MODIFY 修改列的数据类型和位置
```
ALTER TABLE <table name>
MODIFY COLUMN <column name> <new type>;
MODIFY COLUMN <column name> AFTER/BEFORE <column name> FIRST / SECOND / 

```

###  DROP 从表中删除某列
```
ALTER TABLE <table name>
DROP COLUMN <column name>
```

### 删除主键
```
ALTER TABLE <table name>
DROP PRIMARY KEY
```

### 更改表名
```
ALTER TABLE <table name>
RENAME TO <new table name>
```


### 2.8 SELECT
You often use the SELECT statement to query data from one or more table. The syntax of the SELECT statement is as follows:
```
SELECT DISTINCT column_list
FROM table_list
  JOIN table ON join_condition
WHERE row_filter
ORDER BY column DESC/ASC
LIMIT count OFFSET offset
GROUP BY column
HAVING group_filter;
```
```
SELECT
 column_list
FROM
 table
ORDER BY
 column_1 ASC,
 column_2 DESC;
```

```
SELECT COUNT(DISTINCT column name)
FROM table name;
```

#### limit
```
SELECT
 column_list
FROM
 table
LIMIT row_count OFFSET offset;
```

```
SELECT
 column_list
FROM
 table
LIMIT offset, row_count;
```

#### group by
```
SELECT
 column_1,
 aggregate_function (column_1)
FROM
 table
GROUP BY
 column_1, column_2, ...;
```
 

### 2.9 Having
```
SELECT
 column_1,
 aggregate_function (column_2)
FROM
 table
GROUP BY
 column_1
HAVING
 search_condition;
 ```
 example
 ```
 SELECT
 albumid,
 COUNT(trackid)
FROM
 tracks
GROUP BY
 albumid
HAVING albumid = 1;
```

### 2.10 WHERE

The search condition in the WHERE has the following form:
```
left_expression COMPARISON_OPERATOR right_expression
```
For example, you can form a search condition as follows:
```
WHERE column_1 = 100;
 
WHERE column_2 IN (1,2,3);
 
WHERE column_3 LIKE 'An%';
 
WHERE column_4 BETWEEN 10 AND 20;

WHERE column_5 IS NUll;
```

Operator | Meaning
---|---
ALL	| returns 1 if all expressions are 1.
AND	| returns 1 if both expressions are 1, and 0 if one of the expressions is 0.
ANY	| returns 1 if any one of a set of comparisons is 1.
BETWEEN	| returns 1 if a value is within a range.
EXISTS |	returns 1 if a subquery contains any rows.
IN |	returns 1 if a value is in a list of values.
LIKE	| returns 1 if a value matches a pattern
NOT	| reverses the value of other operators such as NOT EXISTS, NOT IN, NOT BETWEEN, etc.
OR	| returns true if either expression is 1
#### like语句中
There are two ways to construct a pattern using percent sign % and underscore _ wildcards:
> The percent sign % wildcard matches any sequence of zero or more characters.

> The underscore _ wildcard matches any single character.

#### GLOB 语句中
The following shows the GLOB wildcards:

The asterisk (*) wildcard matches any number of characters.
The question mark (?) wildcard matches exact one character.
In addition, you can use the list wildcard [] to match one character from a list of characters. For example [xyz] match any single x, y, or z character.

The list wildcard also allows a range of characters e.g., [a-z] matches any single lowercase character from a to z. The [a-zA-Z0-9] pattern matches any single alphanumeric character, both lowercase and uppercase.

You use the ^ at the beginning of the list to match any character except any character in the list. For example, the [^0-9] pattern matches any single character except a numeric character.

### 2.11 UNION
To combine rows from two or more queries into a single result set, you use SQLite UNION clause. The following illustrates the syntax of the SQLite UNION clause:

```
query_1
UNION [ALL]
query_2
UNION [ALL]
query_3
...;
```
Both UNION and UNION ALL clauses combine rows from result sets into a single result set. The UNION clause removes duplicate rows that exist, while the UNION ALL clause does not.

Because the UNION ALL clause does not remove duplicate rows, it processes faster than the UNION clause.

The following are rules to union data:

The number of columns in all queries must be the same.
The corresponding columns must have compatible data type
The column names of the first query determine the column names of the combined result set.
The GROUP BY and HAVING clauses are applied to each individual query, not the final result set.
The ORDER BY clause is applied to the combined result set, not within the individual result set.


### 2.12 函数  functions
#### SQLite aggregate functions

Name|Description
---|---
AVG	|Returns the average value of non-null values in a group
COUNT      |	Return the total number of rows in a table.
MAX	|Returns  the maximum value of all values in a group.
MIN	|Returns the minimum value of all values in a group.
SUM	|Returns the sum of all non-null values in a column

#### SQLite string functions

Name|	Description
---|---
SUBSTR	|Extracts and returns a substring with a predefined length starting at a specified position in a source string.截取固定长度之后的文字。substr(X,Y,Z) function returns a substring of input string X that begins with the Y-th character and which is Z characters long. If Z is omitted then substr(X,Y) returns all characters through the end of the string X beginning with the Y-th
TRIM	|Returns a copy of a string that has specified characters removed from the beginning and the end of a string.
LTRIM	|Returns a copy of a string that has specified characters removed from the beginning of a string.
RTRIM	|Returns a copy of a string that has specified characters removed from the end of a string.
LENGTH	|Returns the number of characters in a string or the number of byte in a BLOB.
REPLACE|	Return a copy of a string with each instance of a substring replaced by the other substring.
UPPER	|Returns a copy of a string with all of the characters converted to uppercase.
LOWER	|Returns a copy of a string with all of the characters converted to lowercase.
INSTR	|Finds a substring in a string and returns an integer indicating the position of the first occurrence of the substring.

####SQLite control flow functions

Name|	Description
---|---
COALESCE|	Returns the first non-null argument
IFNULL	|Provides the NULL if/else construct
NULLIF	|Returns NULL if the first argument is equal to the second argument.

####SQlite date and time functions

Name|	Description
---|---
DATE	|Calculates the date based on multiple date modifiers.
TIME	|Calculates the time based on multiple date modifiers.
DATETIME	|Calculates the date & time based on multiple date and time modifiers.
JULIANDAY	|Returns the Julian day, which is the number of days since noon in Greenwich on November 24, 4714 B.C.
STRFTIME	|Formats the date based on a specified format string.

#### 日期类型数据处理

Using the TEXT storage class for storing SQLite date and time

If you use the TEXT storage class to store date and time value, you need to use the ISO8601 string format as follows:

```
YYYY-MM-DD HH:MM:SS.SSS
```
For example, 2016-01-01 10:20:05.123

First, create a new table named datetime_text for demonstration.
```
CREATE TABLE
IF NOT EXISTS datetime_text (d1 text, d2 text)
INSERT INTO datetime_text (d1, d2)
VALUES
 (
 datetime('now'),
 datetime('now', 'localtime')
 );

```

strftime

```
strftime(‘%Y’,字段)
```

格式 | 说明
---|---
%d|day of month: 00
%f|fractional seconds: SS.SSS
%H|hour: 00-24
%j|day of year: 001-366
%J|Julian day number
%m|month: 01-12
%M|minute: 00-59
%s|seconds since 1970-01-01
%S|seconds: 00-59
%w|day of week 0-6 with Sunday==0
%W|week of year: 00-53
%Y|year: 0000-9999
%%|%



####SQLite math functions

Name|	Description
---|---
ABS	|Returns the absolute value of a number
RANDOM	|Returns a random floating-point value between the minimum and maximum integer values
ROUND	|Round off a floating value to a specified precision.



## 3.多表操作
### 3.1子查询 subquery
子查询可能出现的四个地方:
* SELECT 子句
* column list 作为其中一列
* FROM 子句
* HAVING 子句

* SELECT 子句
```
SELECT column_1
  FROM table_1
 WHERE column_1 = (SELECT column_1 FROM table_2);
```

```
SELECT column_1
  FROM table_1
 WHERE column_1 IN (SELECT column_1 FROM table_2);
```

* column list 作为其中一列
```
SELECT mc.first_name, ma.last_name,
(SELECT state FROM zip_code WHERE mc.zip_code = zip_code) AS state
FROM my_contacts mc;
```
* FROM 子句

```
SELECT avg(album.size) 
  FROM (
           SELECT sum(bytes) size
             FROM tracks
            ORDER BY albumid
       )
       AS album;
```
* HAVING 子句

上面的子查询，都是非关联子查询（noncorrelated subquery），指子查询可以独立运行且不会引用外层查询的任何结果，即称为非关联子查询

关联子查询（correlated subquery），内层查询的解析元需要依赖外层查询的结果

```
SELECT mc.first_name, mc.last_name
FROM my_contacts AS mc
WHERE 3 = (SELECT COUNT(*) FROM contact_interest WHERE contact_id = mc.contact_id);
```




### 3.2表连接
#### 3.2.1 外连接 out join
![](http://www.sqlitetutorial.net/wp-content/uploads/2015/12/SQLite-left-join-example.png)

The following Venn Diagram illustrates the LEFT JOIN clause.
![](http://www.sqlitetutorial.net/wp-content/uploads/2015/12/SQLite-Left-Join-Venn-Diagram.png)

#### 3.2.2 内连接 inner join
##### 相等连接（ equijoin）
内连接的本质是通过查询中的条件移除了某些结果数据行后的交叉联接。

![](http://www.sqlitetutorial.net/wp-content/uploads/2015/12/SQLite-Inner-Join-Example.png)

The following diagram illustrates the INNER JOIN clause:
![](http://www.sqlitetutorial.net/wp-content/uploads/2015/12/SQLite-inner-join-venn-diagram.png)

##### 不等连接（non-equijoin）
```
SELECT	 boys.boy,toys.toy
FROM boss
INNER JOIN
toys
ON boys.toy_id <> toys.boy_id
ORDER BY boys.boy;	
```
##### 自然连接（ natural join）
内连接的两张表的列名相同时，为自然连接
```
SELECT	 boys.boy,toys.toy
FROM boss
NATURAL JOIN
toys;
```

#### 连接三个数据表的用法

```
FROM (Member INNER JOIN MemberSort ON Member.MemberSort=MemberSort.MemberSort) INNER JOIN MemberLevel ON Member.MemberLevel=MemberLevel.MemberLevel
```

#### 3.2.3 交叉连接 cross join
```
CREATE TABLE ranks (
    rank TEXT NOT NULL
);
 
CREATE TABLE suits (
    suit TEXT NOT NULL
);
 
INSERT INTO ranks(rank) 
VALUES('2'),('3'),('4'),('5'),('6'),('7'),('8'),('9'),('10'),('J'),('Q'),('K'),('A');
 
INSERT INTO suits(suit) 
VALUES('Clubs'),('Diamonds'),('Hearts'),('Spades');
```

```
SELECT rank,
       suit
  FROM ranks
       CROSS JOIN
       suits
ORDER BY suit;
```

#### 3.2.4 自连接
The self-join is special kind of join that allows you to join a table to itself using either LEFT JOIN or INNER JOIN clause. You use self-join to create a result set that joins the rows with the other rows within the same table.

Because you cannot refer to the same table more than one in a query, you need to use a table alias to assign the table a different name when you use self-join.

The self-join compares values of the same or different columns in the same table. Only one table is involved in the self-join.

You often use self-join to query parents / child relationship stored in a table or to obtain running totals.

```
SELECT DISTINCT
 e1.city,
 e1.firstName || ' ' || e1.lastname AS fullname
FROM
 employees e1
INNER JOIN employees e2 ON e2.city = e1.city 
   AND (e1.firstname <> e2.firstname AND e1.lastname <> e2.lastname)
ORDER BY
 e1.city;
```
 
## 4.database objects
### VIEW
What is a view
In database theory, a view is a result set of a stored query. A view is the way to pack a query into a named object.
A view is useful in some cases:

First, views provide an abstraction layer over tables. You can add and remove the columns in the view without touching the schema of the underlying tables.
Second, you can use views to encapsulate complex queries with joins to simplify the data access.
SQLite view is read only. It means you cannot use INSERT, DELETE, and UPDATE statement to update data in the base tables through the view.

```
CREATE [TEMP] VIEW [IF NOT EXISTS] view_name(column-name-list)
AS 
   select-statement;
```

example

```
CREATE VIEW v_tracks 
AS 
SELECT
 trackid,
 tracks.name,
 albums.Title AS album,
 media_types.Name AS media,
 genres.Name AS genres
FROM
 tracks
INNER JOIN albums ON Albums.AlbumId = tracks.AlbumId
INNER JOIN media_types ON media_types.MediaTypeId = tracks.MediaTypeId
INNER JOIN genres ON genres.GenreId = tracks.GenreId;
```


### 事务 transaction
SQLite guarantees all the transactions are ACID compliant even if the transaction is interrupted by program crash, operation system dump, or power failure to the computer.

* **Atomic**: a transaction should be atomic. It means that a change cannot be broken down into smaller ones. When you commit a transaction, either entire the transaction is applied or not applied. It cannot be only part of the transaction to be applied .
* **Consistent**: a transaction must ensure to change the database from one valid state to another. When a transaction starts and executes statements to modify data, the database becomes inconsistent. However, when the transaction is committed or rolled back, it is important that the transaction must keep the database consistent.
* **Isolation**: a pending transaction must be isolated from other clients. When a client starts a transaction and executes the INSERT or UPDATE statement to change the data, those changes are only visible to the client, not other clients. On the other hand, the changes committed by other clients after the transaction started should not be visible to this client.
* **Durable**: if a transaction is successfully committed, the changes must be permanent in the database regardless of the condition such as power failure or program crash. On the contrary, if the program crash before the transaction is committed, the change should not be present.

By default, SQLite is in auto-commit mode. It means that for each command, SQLite starts, processes, and commits the transaction automatically.

To start a transaction explicitly, you use the following steps:

First, open a transaction by issuing the BEGIN TRANSACTION command.

```
BEGIN TRANSACTION;
```
After executing the BEGIN TRANSACTION the transaction is open until it is explicitly committed or rolled back.

Second, issue the SQL commands to select or update data in the database. Note that the change is only visible to the client.

Third, to commit the changes to the database, you use the COMMIT or COMMIT TRANSACTION statement.

```
COMMIT;
```
For any reason, if you do not want to commit the transaction, you can roll it back using the ROLLBACK or ROLLBACK TRANSACTION statement.

```
ROLLBACK;
```


### INDEX
## 5.用户管理
## 6.SQLITE & PYTHON
## 0.数据库操作
### 创建数据库

```
CREAT DATEBASE <数据库名>;
```

### 打开数据库
```
c:\sqlite>sqlite3 db/chinook.db
```

### 退出
```
.quit
```
### 帮助
```
.help
```
### 显示数据库
```
.database
```
### 显示表
```
.tables
```
### Show the structure of a table
```
sqlite> .schema <tables>
```
### Save the result of a query into a file

To save the result of a query into a file, you use the .output FILENAME command. Once you issue the .output command, all the results of the subsequent queries will be saved to the file that you specified in the FILENAME argument. If you want to save the result of the next single query only to the file, you issue the .once FILENAME command.

To display the result of the query to the standard output again, you issue the .output command without arguments.

The following commands select the title from the albums table and write the result to the albums.txt file.

```
sqlite> .output albums.txt
sqlite> SELECT title FROM albums;
```

### SQL statements from a file

Suppose we have a file named commands.txt in the c:/sqlite/ folder with the following content:
```
SELECT albumid, title
FROM albums
ORDER BY title
LIMIT 10;
```
To execute the SQL statements in the commands.txt file, you use the .read FILENAME command as follows:

```
sqlite> .mode column
sqlite> .read c:/sqlite/commands.txt
```

### DUMP
Dump the entire database into a file using SQLite dump command

The following command opens a new SQLite database connection to the chinook.db file.
```
C:\sqlite>sqlite3 c:/sqlite/chinook.db
```

To dump a database into a file, you use the .dump command. The .dump command converts the entire structure and data of an SQLite database into a single text file.

By default, the .dump command outputs the SQL statements on screen. To issue the output to a file, you use the  .output FILENAME command.

The following commands specify the output of the dump file to chinook.sql and dump the chinook database into the chinook.sql file.

```
sqlite> .output c:/sqlite/chinook.sql
sqlite> .dump
sqlite> .exit
```


### 导入csv文件

First, set the mode to CSV to instruct the command-line shell program to interpret the input file as a CSV file. To do this, you use the .mode command as follows:

```
sqlite> .mode csv
```
Second, use the .import FILE TABLE command to import the data from the city.csv file into the cities table.
```
sqlite>.import c:/sqlite/city.csv cities
```
To verify the import, you use the .schema command to display the structure of the cities table.
```
sqlite> .schema cities
CREATE TABLE cities(
  "name" TEXT,
  "population" TEXT
);
```
To view the data of the cities table, you use the following SELECT statement.
```
SELECT name, 
       population
FROM cities;
```
In the second scenario, the table is already available in the database and you just need to import the data.
First, remove the cities table that you have created.
```
DROP TABLE IF EXISTS cities;
```
Second, use the following CREATE TABLE statement to create the cities table.
```
CREATE TABLE cities(
  name TEXT NOT NULL,
  population INTEGER NOT NULL 
);
```
If the table already exists, the sqlite3 tool uses all the rows, including the first row, in the CSV file as the actual data to import. Therefore, you should delete the first row of the CSV file.

The following commands import the city_without_header.csv file into the cities table.
```
sqlite> .mode csv
sqlite> .import c:/sqlite/city_no_header.csv cities
```
其他方法[http://www.sqlitetutorial.net/sqlite-import-csv/](http://www.sqlitetutorial.net/sqlite-import-csv/)

### Export CSV
To export data from the SQLite database to a CSV file, you use these steps:

Turn on the header of the result set using the .header on command.
Set the output mode to CSV to instruct the sqlite3 tool to issue the result in the CSV mode.
Send the output to a CSV file.
Issue the query to select data from the table to which you want to export.
The following commands select data from the customers table and export it to the data.csv file.

```
>sqlite3 c:/sqlite/chinook.db
sqlite> .headers on
sqlite> .mode csv
sqlite> .output data.csv
sqlite> SELECT customerid,
   ...>        firstname,
   ...>        lastname,
   ...>        company
   ...>   FROM customers;
sqlite> .quit
```



