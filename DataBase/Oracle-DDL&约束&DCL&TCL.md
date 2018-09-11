---
title: Oracle | DDL&约束&DCL&TCL
comments: true
date: 2018-05-13 17:32:29
id: db-oracle-ddl
tags:
- Oracle
- Database
categories: DataBase
toc: true
reward: true
copyright: true
---

<!--# Oracle | DDL&约束&DCL&TCL-->

- DDL（Data Definition Language，数据定义语言），用于创建/修改/删除数据库的各种对象，如表、视图、索引、同义词、序列等。DDL语句主要包括 `CREATE` 创建、`ALTER` 修改、`TRUNCATE` 截断 、`DROP` 删除。
- 约束CONSTRAINT，用于限制表的数据的类型，主要包括非空约束 `NOT NULL`、唯一约束 `UNIQUE`、主键约束 `PRIMARY KEY`、检查约束 `CHECK`、外键约束 `FOREIGN KEY`。
- DCL（Data Control Language，数据控制语言），用于授予/回收访问数据库的权限。DCL 语句即`GRANT` 授予、`REVOKE` 回收。
- TCL（Transaction Control Language，事务控制语言），用于事务的提交/回滚。TCL语句包括 `COMMIT` 提交、`ROLLBACK` 回滚，`SAVEPOINT` 保存点。

<!--more-->

# Oracle数据库

## 解锁账户

```sql
alter user <账户名> account unlock;
```

## Oracle 数据类型

- NUMBER：整数
- DATE：日期
- CLOB：大文本数据，可存4G文本数据
- BLOB：二进制数据，可存图片、音频、视频、文字，最大4G。
- VARCHAR：固定长度字符串，标准sql数据类型
- VARCHAR2：可变长度字符串，oracle特有。
  - 字符串：`'abc'`
  - 字符串连接符：`||`

## 数据伪列

- `ROWID`：数据库中每一行都有一个行地址，rowid伪列返回该行地址。rowId值可以唯一标识数据库中的一行。
- `ROWNUM`：返回一个数值代表行的次序。通过使用rownum，用户可以限制查询返回的行数。

# DDL

数据定义语言DDL，主要用于创建/修改/删除数据库的各种对象，如表、视图、索引、同义词、序列等。DDL语句主要包括 `CREATE` 创建、`ALTER` 修改、`DROP` 删除。

## 表：TABLE

### 创建表

```mysql
CREATE TABLE <表名>(
    <列名> <数据类型> [<DEFAULT 默认值>] [<约束>],
    <列名> <数据类型> [<DEFAULT 默认值>] [<约束>],
    ...
    [CONSTRAINT <约束名> <约束类型>(约束列)]
)
```

### 修改表

修改表中列

1.增加列：

```mysql
ALTER TABLE <表名> ADD （
    <列名> <数据类型> [<DEFAULT 默认值>] [<列级约束>],
    <列名> <数据类型> [<DEFAULT 默认值>] [<列级约束>],
    ...
)
```

2.修改列：

```mysql
ALTER TABLE <表名> MODIFY （
    <列名> <数据类型> [<DEFAULT 默认值>] [<列级约束>],
    <列名> <数据类型> [<DEFAULT 默认值>] [<列级约束>],
    ...
)
```

3.删除列：

```mysql
ALTER TABLE <表名> DROP COLUMN <列名>
```

4.重命名列：

```mysql
ALTER TABLE <表名> RENAME <旧名> TO <新名>
```

修改表中属性：

1.添加约束：

```mysql
ALTER TABLE <表名> ADD CONSTRAINT <约束名> <约束类型>(约束列);
```

2.删除约束：

```mysql
ALTER TABLE <表名> DROP CONSTRAINT <约束名>;
```

### 截断表

```sql
TRUNCATE TABLE <表名>
```

### 删除表

1.删除表：

```sql
DROP TABLE <表名>
```

2.截断表：

### 复制表

1.复制表的结构与数据：

```mysql
CREATE TABLE <表名> AS 
	SELECT <目标列表达式>
	FROM <表>
	WHERE <查询条件>
```

2.仅复制表的结构：`WHERE 1 = 2`

```mysql
CREATE TABLE <表名> AS 
	SELECT <目标列表达式>
	FROM <表>
	WHERE 1 = 2;
```

### 重命名

```mysql
RENAME <旧名称> TO <新名称>
```

## 视图：VIEW

视图 (VIEW)：虚拟表，不占物理空间。

### 创建视图

```mysql
CREATE [OR REPLACE] [FORCE] VIEW <视图名>
[(列名1, 列2,...)]
AS 
SELECT...
[WITH CHECK OPTION]
[WITH READ ONLY];
```

> FORCE：强制创建视图
> WITH CHECK OPTION：检查约束视图
> WITH READ ONLY：只读视图

### 删除视图

```mysql
DROP VIEW <视图名>
```

## 索引：INDEX

索引：加快查询速度

### 创建索引

```mysql
CREATE [UNIQUE] INDEX <索引名>
ON <表名> (<列名> [ASC | DESC], ... )
[INITRANS n]
[MAXTRANS n]
[PCTFREE n]
[TABLESPACE <表空间名>]
[STORAGE storage]
[NOSORT]
```

> UNIQUE：唯一索引
> INITRANS：初始事务入口数
> MAXTRANS：最大事务入口数
> PCTFREE：索引数据块空闲空间的百分比 
> NOSORT：不排序

###修改索引

重建索引

```mysql
ALTER INDEX <索引名> REBUILD;
```

合并索引

```mysql
ALTER INDEX <索引名> COALESCE;
```

### 删除索引

```mysql
DROP INDEX <索引名>
```



## 同义词：SYNONYM 

同义词：数据库对象的一个别名，操作权限不变，简化对象访问。

### 创建同义词

```mysql
CREATE [PUBLIC] SYNONYM <表名同义词>
FOR <用户>.<表名>
```

### 删除同义词

```mysql
DROP [PUBLIC] SYNONYM <同义词>
```

## 序列：SEQUENCE 

序列 (SEQUENCE)： 序列号生成器，为表中的行自动生成序列号，产生一组等间隔的数值(类型为数字)。不占用磁盘空间，占用内存。 

其主要用途是生成表的主键值，可以在插入语句中引用，也可以通过查询检查当前值，或使序列增至下一个值。

### 创建序列 

```mysql
CREATE SEQUENCE <序列名>
[INCREMENT BY n]
[START WITH n]
[MAXVALUE n | NOMAXVALUE]
[MINVALUE n | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE n | NOCACHE];
```

> INCREMENT BY：定义步长
> START WITH：定义序列初始值
> MAXVALUE / MINVALUE：定义序列最大值/最小值
> NOMAXVALUE：默认最值
> CYCLE / NOCYCLE：表示序列达到限定值后是否循环
> CACHE n：对序列进行内存缓冲，提前生成n个序号存入缓存，默认n=20
> NOCACHE：不对序列进行内存缓冲

### **序列的两个伪列**

- NEXTVAL：序列中的下一个有效值
- CURRVAL：序列当前值

### 修改序列

```mysql
ALTER SEQUENCE [<用户>.]<序列名>
[INCREMENT BY n]
[START WITH n]
[MAXVALUE n | NOMAXVALUE]
[MINVALUE n | NOMINVALUE]
[CYCLE | NOCYCLE]
[CACHE n | NOCACHE];
```

### 删除序列

```mysql
DROP SEQUENCE <序列名>
```



# 约束-CONSTRAINT

约束CONSTRAINT，用于限制表的数据的类型。

```mysql
CONSTRAINT <约束名> <约束类型>(约束列)
```

## 非空约束：NOT NULL

非空约束：NOT NULL

```sql
<列名> <数据类型> NOT NULL
```

## 唯一约束：UNIQUE

唯一约束：UNIQUE

```mysql
<列名> <数据类型> UNIQUE
```

```mysql
CONSTRAINT uk_cloumn UNIQUE(<列名>);
```

## 主键约束：PRIMARY KEY

主键约束：PRIMARY KEY，非空+唯一

```mysql
CONSTRAINT pk_cloumn PRIMARY KEY(<列名>);
```

联合主键：

```mysql
CONSTRAINT pk_cloumn PRIMARY KEY(<列名1>,<列名2>,...);
```

## 检查约束：CHECK

```mysql
CONSTRAINT ck_cloumn CHECK(<列名> IN (值1,值2...))
```

```mysql
CONSTRAINT ck_cloumn CHECK(<列名> BETWEEN 值1 AND 值2)
```

## 外键约束：FOREIGN KEY

```mysql
CONSTRAINT fk_cloumn FOREIGN KEY (<列名1>,<列名2>...) REFERENCES <表名>(<列名1>,<列名2>...) ON DELETE <CASCADE|SET NULL> 
```

# 数据库安全-DCL

数据控制语言DCL，用于授予/回收用户/角色访问数据库的权限，DCL语句即 `GRANT` 授予、`REVOKE` 回收。

## 授予权限：GRANT

```mysql
GRANT <权限>
ON <对象类型> <对象名>
TO <用户>
[WITH GRANT OPTION]
```

> WITH GRANT OPTION：可传播权限，即将该权限再授予其他用户。

## 收回权限：REVOKE

```mysql
REVOKE <权限>
ON <对象类型> <对象名>
FROM <用户> [CASCADE|RESTRICT]
```

>CASCADE：级联
>RESTRICT：限定

## 角色：ROLE

角色(ROLE)：一组数据库操作权限的集合。

**1.创建角色**：

```mysql
CREATE ROLE <角色名>
```

**2.为角色授权**：

```mysql
GRANT <权限>
ON <对象类型> <对象名>
TO <角色>
```

**3.将角色授权给其他角色或用户**：

```mysql
GRANT <角色1>
ON <对象类型> <对象名>
TO <角色2>,<用户>
```
# 事务-TCL

**事务**：用于**保证数据完整性**。由一组DML语句组成，这组DML语句要么全部成功，要么全部失败。

### 事务特性：ACID

- **原子性（atomicity）**：一个事务是一个不可分割的工作单位，事务中的一组操作要么全做，要么全不做。
- **一致性（consistency）**：事务必须是使数据库从一个一致性状态变到另一个一致性状态。
- **隔离性（isolation）**：一个事务的执行不能被其他事务干扰。即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
- **持久性（durability）**：指一个事务一旦提交，它对数据库中数据的改变就应该是永久性的。接下来的其他操作或故障不应该对其有任何影响。

### 事务锁

**事务并发不一致性**导致的问题：

- **幻读**：事务T1读取一条指定条件的语句，返回结果集。此时事务T2插入一行新记录并commit，恰好满足T1的条件。然后T1使用相同的条件再次查询，结果集中可以看到T2插入的记录，这条新纪录就是幻想。（事务T1查询，并行事务T2插入/删除部分数据并提交，事务T1再次查询，数据发生改变）
- **不可重复读**：事务T1读取一行记录，紧接着事务T2修改了T1刚刚读取的记录并commit，然后T1再次查询，发现与第一次读取的记录不同，这称为不可重复读。 （事务T1读取一条记录，并行事务T2修改了该记录，事务T1再次查询，数据与第一次读的不同）
- **脏读**：一个事务读取了另一个未提交的并行事务写的数据。事务T1更新了一行记录，还未提交所做的修改，这个T2读取了更新后的数据，然后T1执行回滚操作，取消刚才的修改，所以T2所读取的行就无效，也就是脏数据。（事务T1更新但未提交，事务T2读取到更新后的数据，事务T1回滚，事务T2读取无效）
- **丢失更新**：当两个或多个事务选择同一数据，并且基于最初选定的值更新该数据时，会发生丢失更新问题。（事务T1读取数据，并行事务T2读取同一数据，事务T1更新并提交，事务T2更新并提交，事务T2覆盖事务T1提交结果）

--于是引入事务锁，以保证数据的完整性。

**四种隔离级别**：读未提交 < 读已提交 < 可重复读 < 序列化

- 读未提交：一个事务可以读取另一个未提交事务的数据。
- 读已提交：一个事务必须等另一个事务提交后才能读取数据。（Oracle，SQL Server默认）
- 可重复读：在开始读取数据（事务开启）时，不再允许修改操作。（MySQL默认）
- 串行化：事务串行化顺序执行。效率低下，一般不使用。

## 事务处理-TCL

事务控制语言TCL，用于事务的提交/回滚。TCL语句即 `COMMIT` 提交、`ROLLBACK` 回滚，`SAVEPOINT` 保存点。

### 提交：COMMIT

事务提交 `COMMIT`：将事务中对数据库的修改进行永久保存。

- 显式提交：需要主动提交SQL语句对于数据库的修改，未提交之前可以rollback。如DML操作。
- 隐式提交：SQL语句执行结束自动提交，无法rollback。如DDL，DCL。

### 回滚：ROLLBACK

回滚 `ROLLBACK`：取消事务中对数据库进行的修改。

### 保存点：SAVEPOINT

```sql
savepoint <savepoint_name> 
```

### 回滚至保存点

```sql
rollback to savepoint <savepoint_name>
```

# 数据库设计范式

实际设计原则：根据业务尽可能的**减少多表查询**，不必完全遵循设计范式。

**第一范式**：确保表中每个字段都不可拆分。

**第二范式**：消除了部分函数依赖；确保表中每列都与主键相关（主要针对联合主键）。

**第三范式**：消除了传递函数依赖；确保每列与主键直接相关，而不是间接相关。





