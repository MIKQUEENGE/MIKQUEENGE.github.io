---
title: 常用SQL语句
toc: true
date: 2018-09-29 11:55:20
categories:
- Web
tags:
- 数据库
- SQL
---

## 一些最重要的 SQL 命令

- **SELECT** - 从数据库中提取数据
- **UPDATE** - 更新数据库中的数据
- **DELETE** - 从数据库中删除数据
- **INSERT INTO** - 向数据库中插入新数据
- **CREATE DATABASE** - 创建新数据库
- **ALTER DATABASE** - 修改数据库
- **CREATE TABLE** - 创建新表
- **ALTER TABLE** - 变更（改变）数据库表
- **DROP TABLE** - 删除表
- **CREATE INDEX** - 创建索引（搜索键）
- **DROP INDEX** - 删除索引

### SELECT

```sql
SELECT column1, column2, ...
FROM table_name;
```

这里，column1，column2，...是要从中选择数据的表的字段名称。如果要选择表中可用的所有字段，请使用以下语法：

```sql
SELECT * FROM table_name;
```

**SELECT DISTINCT**语法用于仅返回不同的（different）值。

```sql
SELECT DISTINCT column1, column2, ...
FROM table_name;
```

可以用**COUNT**获取不同值的数量：

```sql
SELECT COUNT(DISTINCT Country) FROM Customers;
```

### WHERE

**WHERE**子句用于提取满足指定标准的记录，WHERE子句不仅用于SELECT语法，还用于UPDATE，DELETE语法等。

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

例如：

```sql
SELECT * FROM Customers
WHERE Country='Mexico';
```

WHERE子句中可以使用以下运算符：

| 运算符  | 描述                                                        |
| ------- | ----------------------------------------------------------- |
| =       | 等于                                                        |
| <>      | 不等于。 **注意**：在某些版本的SQL中，这个操作符可能写成！= |
| >       | 大于                                                        |
| <       | 小于                                                        |
| >=      | 大于等于                                                    |
| <=      | 小于等于                                                    |
| BETWEEN | 在某个范围内                                                |
| LIKE    | 搜索某种模式                                                |
| IN      | 为列指定多个可能的值                                        |

### AND & OR & NOT

WHERE子句可以与AND，OR和NOT运算符结合使用。

AND和OR运算符用于根据多个条件筛选记录：

- 如果由AND分隔的所有条件为TRUE，则AND运算符显示记录。
- 如果由OR分隔的任何条件为真，则OR运算符显示记录。

如果条件不为真，则利用NOT运算符显示记录。 

**AND**语法

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;
```

**OR**语法

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
```

**NOT**语法

```sql
SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
```

### ORDER BY

ORDER BY 关键字用于按升序或降序对结果集进行排序。

ORDER BY 关键字默认情况下按升序排序记录。

如果需要按降序对记录进行排序，可以使用DESC关键字。

```sql
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;
```

例如：

```sql
SELECT * FROM Customers
ORDER BY Country DESC;
```

### UPDATE

UPDATE 语句用于更新表中已存在的记录。 

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

WHERE子句指定哪些记录需要更新。如果省略WHERE子句，所有记录都将更新！

栗子：

```sql
UPDATE Customers
SET ContactName='Juan'
WHERE Country='Mexico';
```

### DELETE

DELETE 语句用于删除表中的行。

```sql
DELETE FROM table_name
WHERE condition;
```

WHERE子句指定需要删除哪些记录。如果省略了WHERE子句，表中所有记录都将被删除！

栗子：

```sql
DELETE FROM Customers
WHERE CustomerName='Alfreds Futterkiste';
```

### INSERT INTO

INSERT INTO 语句用于向表中插入新记录。

INSERT INTO 语句可以用两种形式编写。

指定要插入数据的列的名称，提供要插入的值：

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

如果要为表中的所有列添加值，则不需要在SQL查询中指定列名称。但是，请确保值的顺序与表中的列顺序相同。INSERT INTO语法如下所示：

```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

### 空值

**NULL**用于表示缺失的值。

数据表中的 NULL 值表示该值所处的字段为空。

具有NULL值的字段是没有值的字段。

**如何测试NULL值？**

使用比较运算符（例如=，<或<>）来测试NULL值是不可行的。

我们将不得不使用IS NULL和IS NOT NULL运算符。

**IS NULL语法**

```sql
SELECT column_names
FROM table_name
WHERE column_name IS NULL;
```

**IS NOT NULL语法**

```sql
SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL;
```

更多的可以看：

https://www.cnblogs.com/acpe/p/4970765.html

https://www.w3cschool.cn/sql/dlwiyfom.html