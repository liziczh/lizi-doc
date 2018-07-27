---
title: Database | DQL&DML
comments: true
date: 2018-05-12 19:32:29
id: db-dml
tags:
- Oracle
- Database
categories: DataBase
toc: true
reward: true
copyright: true
---

<!--# Database | DQL&DML-->

SQL（Structured Query Language，结构化查询语言），面向集合的描述性非过程化语言，用于访问和处理关系数据库的标准语言。
SQL 是一种 ANSI 标准，所以存在多种不同版本的 SQL 语言。但就 SQL 查询和更新而言，Oracle、MySql 等数据库系统的实现大同小异，基本可以互通移植。

- DQL（Data Query Language，数据查询语言），即 SQL 查询，DQL 语句即 `SELECT` 查询块。
- DML（Data Manipulation Language，数据操作语言），即 SQL 更新，DML 语句即 `INSERT` 插入、`UPDATE` 更新、`DELETE` 删除。

<!--more-->

## 1. SQL查询★

SELECT 语句的一般格式：

```sql
SELECT [DISTINCT] <目标列表达式> [AS] <别名>
FROM <表/视图> [AS] <别名>
[WHERE <查询条件>]
[GROUP BY <分组列> [HAVING <分组条件>]]
[ORDER BY <排序列> [ASC|DESC]];
```

### 1.1 单表查询

#### 1.1.1 简单查询：SELECT

```sql
SELECT [DISTINCT] <目标列表达式> [AS] <别名>
FROM <表/视图> [AS] <别名>
```

1.查询全部列：使用通配符`*`实现查询全部。

```sql
SELECT *
FROM <表/视图>
```

2.去除重复行：`DISTINCT`

```sql
SELECT DISTINCT <目标列>
FROM <表/视图>
```

3.指定别名（Alias）：

```mysql
<列名/表名> <别名>
<列名/表名> AS <别名>
```

#### 1.1.2 查询条件：WHERE

```sql
SELECT <目标列表达式> [AS] <别名>
FROM <表/视图> [AS] <别名>
WHERE <查询条件>
```

**查询条件**：

1.比较运算：`>`，`<`，`=`，`>=`，`<=`，`!=`/`<>`

2.确定范围：`BETWEEN...AND...`

```sql
<列名> [NOT] BETWEEN <下限> AND <上限>
```

3.确定集合：`IN`

```sql
<列名> [NOT] IN （值1, 值2...）
```

4.模式匹配：`LIKE`

```sql
<列名> [NOT] LIKE '<匹配模式>'
```

| 通配符                         | 描述                         |
| ------------------------------ | ---------------------------- |
| `%`                            | 替代一个或多个字符           |
| `_`                            | 替代一个字符                 |
| `[charlist]`                   | 字符序列中的任何单一字符     |
| `[!charlist]`<br>`[^charlist]` | 不在字符序列中的任何单一字符 |

5.空值：`IS NULL`

```sql
<列名> IS [NOT] NULL
```

6.多重条件（逻辑运算）：`AND`，`OR`，`NOT`

```sql
<条件表达式> AND <条件表达式>
<条件表达式> OR <条件表达式>
NOT <条件表达式>
```

> AND优先级>OR优先级

#### 1.1.3 查询排序：ORDER BY

ORDER BY 子句将查询结果按指定列进行**升序 (ASC)** 或**降序 (DESC)** 排序。

```mysql
ORDER BY <排序列> [ASC|DESC]
```

> ORDER BY 子句只能对最终查询结果排序。不能对内层查询使用。

#### 1.1.4 聚集函数

| 聚集函数                  | 描述           |
| ------------------------- | -------------- |
| `COUNT(*)`                | 统计记录行数   |
| `COUNT([DISTINCT]<列名>)` | 统计列中值个数 |
| `SUM([DISTINCT]<列名>)`   | 计算列值总和   |
| `AVG([DISTINCT]<列名>)`   | 计算列值平均值 |
| `MAX([DISTINCT]<列名>)`   | 求列值最大值   |
| `MIN([DISTINCT]<列名>)`   | 求列值最小值   |

> 注意：
> ①只有COUNT(*)计算空值，其余聚集函数都跳过空值。
> ②WHERE 子句中不能用聚集函数，聚集函数只能用于 SELECT 子句和 GROUP BY 中的 HAVING 子句。

#### 1.1.5 分组查询：GROUP BY

GROUP BY 子句将查询结果按某一列或多列的值分组，值相等的为一组。

```sql
GROUP BY <分组列> HAVING <分组条件>
```

> 分组的目的是为了细化聚集函数的作用对象，分组后聚集函数将作用于每一个组，即每一组都有一个聚集函数值。

**WHERE 子句与 HAVING 子句区别**：
①WHERE 子句作用于基本表/视图，不能使用聚集函数。
②HAVING 子句作用于组。



### 1.2 连接查询

连接查询：使用连接运算符实现多表查询 

#### 1.2.1 交叉连接

交叉连接：笛卡儿积。

①隐式连接：

```sql
SELECT <目标列>
FROM <表1>, <表2>
```

②使用 `JOIN` 连接：

```mysql
SELECT <目标列>
FROM <表1> CROSS JOIN <表2>
```

> 消除笛卡尔积：使用关联字段。 

#### 1.2.2 内连接

内连接：查询与连接条件匹配的所有行，但不去除重复属性列。

①隐式连接：

```sql
SELECT <目标列>
FROM <表1>, <表2>
WHERE <表1>.<关联属性> <比较运算符> <表2>.<关联属性>
```

②使用 `JOIN` 连接：

```mysql
SELECT <目标列>
FROM <表1> [INNER] JOIN <表2> 
ON <表1>.<关联属性> <比较运算符> <表2>.<关联属性> 
```

**1.等值连接**

等值连接：当比较运算符为`=`时的内连接，不去除重复属性列。

①隐式连接：

```sql
SELECT <目标列>
FROM <表1>, <表2>
WHERE <表1>.<关联属性> = <表2>.<关联属性>
```

②使用 `JOIN` 连接：

```mysql
SELECT <目标列>
FROM <表1> [INNER] JOIN <表2> 
ON <表1>.<关联属性> = <表2>.<关联属性>
```

**2.非等值连接**

非等值连接：当比较运算符**不为**`=`时的内连接，不去除重复属性列。

①隐式连接：

```sql
SELECT <目标列>
FROM <表1>, <表2>
WHERE <表1>.<关联属性> <比较运算符> <表2>.<关联属性>
```

②使用 `JOIN` 连接：

```mysql
SELECT <目标列>
FROM <表1> [INNER] JOIN <表2> 
ON <表1>.<关联属性> <比较运算符> <表2>.<关联属性>
```

3.自身连接

自身连接：一个表与其自身进行等值连接。

①隐式连接：

```sql
SELECT <目标列>
FROM <表> FIR, <表2> SEC
WHERE FIR.<关联属性> = SEC.<关联属性>
```

②使用 `JOIN` 连接：

```mysql
SELECT <目标列>
FROM <表> FIR [INNER] JOIN <表2> SEC
ON FIR.<关联属性> = SEC.<关联属性>
```

4.自然连接

自然连接：去除重复属性列的等值连接，消除了笛卡儿积。

```mysql
SELECT *
FROM <表1> NATURAL JOIN <表2>
```

#### 1.2.3 外连接

外连接：主表内容全部显示。未匹配到，用NULL填充。

**1.左外连接**

左外连接：以左表为主表，右表为从表。即使右表中没有匹配，也返回左表所有行。

```mysql
SELECT <目标列>
FROM <表1> LEFT [OUTER] JOIN <表2>
ON <表1>.<关联属性> = <表2>.<关联属性>
```

**Oracle特有写法**：从表以 `(+)` 表示。

```mysql
SELECT <目标列>
FROM <表1>, <表2>
WHERE <表1>.<关联属性> = <表2>.<关联属性>(+)
```

**2.右外连接**

右外连接：以右表为主表，左表为从表。即使左表中没有匹配，也返回右表所有行。

```mysql
SELECT <目标列>
FROM <表1> RIGHT [OUTER] JOIN <表2>
ON <表1>.<关联属性> = <表2>.<关联属性>
```

**Oracle特有写法**：从表以 `(+)` 表示。

```plsql
SELECT <目标列>
FROM <表1>, <表2>
WHERE <表1>.<关联属性>(+) = <表2>.<关联属性>
```

**3.全外连接**

全外连接：返回左表和右表中的所有数据。未匹配字段显示为NULL。

```mysql
SELECT <目标列>
FROM <表1> FULL [OUTER] JOIN <表2>
ON <表1>.<关联属性> = <表2>.<关联属性>
```

#### 1.2.4 指定关联字段

①隐式连接-使用WHERE子句指定关联字段：

```sql
SELECT <目标列>
FROM <表1>, <表2>
WHERE <表1>.<关联属性> <比较运算符> <表2>.<关联属性>
```

②JOIN连接-使用`ON`指定关联字段：

```sql
SELECT <目标列>
FROM <表1> JOIN <表2> 
ON <表1>.<关联属性> <比较运算符> <表2>.<关联属性>
```

③JOIN连接-使用`USING`指定关联字段：

```mysql
SELECT <目标列>
FROM <表1> JOIN <表2> 
USING (<关联属性>)
```

> USING等价于ON指定**等值同名**的关联字段。

### 1.3 子查询

查询块：临时表

```sql
SELECT <目标列>
FROM <表>
WHERE <查询条件>
```

#### 1.3.1 嵌套查询

嵌套查询：将一个查询块嵌套在另一个查询块的WHERE/HAVING条件子句中。

```sql
SELECT <目标列>
FROM <表>
WHERE <列名> <运算符/谓词> 
	(SELECT <目标列>
	 FROM <表>
	 WHERE <查询条件>)
```

**1.带有比较运算符的子查询**

当子查询返回单值时，可以使用**比较运算符**连接。

```sql
SELECT <目标列>
FROM <表>
WHERE <列名> <比较运算符>
	(SELECT <目标列>
	 FROM <表>
	 WHERE <查询条件>)
```

> 比较运算符：`>`，`<`，`>=`，`<=`，`!=`，`<>`，`=`

**2.带有IN的子查询**

当子查询返回一个集合时，一般使用**IN**进行连接。IN在嵌套查询中最为常用。

```sql
SELECT <目标列>
FROM <表>
WHERE <列名> IN
	(SELECT <目标列>
	 FROM <表>
	 WHERE <查询条件>)
```

**3.带有ANY或ALL的子查询**

```plsql
SELECT <目标列>
FROM <表>
WHERE <列名> <比较运算符>ANY
	(SELECT <目标列表达式>
	 FROM <表>
	 WHERE <查询条件>)
```

> <比较运算符>ALL：与子查询结果的**所有值**进行比较运算。
> <比较运算符>ANY：与子查询结果的**任意某个值**进行比较运算。

#### 1.3.2 派生查询

派生查询：将一个查询块嵌套在另一个查询块的FROM子句中，子查询生成的派生表成为了主查询的查询对象。

```sql
SELECT <目标列>
FROM <表>, (SELECT <目标列> FROM <表> WHERE <查询条件>) [AS] <别名>
WHERE <查询条件>
```

> 注意：必须为子查询生成的派生表指定别名。

### 1.4 集合查询

集合查询：多个查询结果进行集合运算。

```sql
SELECT <目标列> FROM <表> WHERE <查询条件>
	<集合运算谓词>
SELECT <目标列> FROM <表> WHERE <查询条件>
```

> 注意：参加集合操作的**各查询结果的列数**必须相同，**对应项的数据类型**也必须相同。

| 集合运算         | 谓词        |
| ---------------- | ----------- |
| 并集（去重复）   | `UNION`     |
| 并集（不去重复） | `UNION ALL` |
| 交集             | `INTERSECT` |
| 差集             | `MINUS`     |



## 2. SQL更新

### 2.1 SQL插入：INSERT

```sql
INSERT INTO <表> (列名, 列名...) VALUES(值, 值...)
```

INSERT简写-按表的列顺序插入一行数据：

```sql
INSERT INTO <表> VALUES(值, 值...)
```

### 2.2 SQL修改：UPDATE

```sql
UPDATE <表> SET 列名 = 值, 列名 = 值... 
[WHERE <修改条件>]
```

### 2.3 SQL删除：DELETE

```sql
DELETE FROM <表> 
[WHERE <删除条件>]
```

