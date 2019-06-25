---
title: Oracle | PL/SQL编程
comments: true
date: 2018-05-15 17:19:29
id: oracle-plsql
tags:
- Oracle
- Database
categories: DataBase
toc: true
reward: true
copyright: true
---

<!--# Oracle | （三）PL/SQL编程-->

PL/SQL（Procedural Language/SQL，过程化SQL语言），是一种高级数据库程序设计语言，专门用于在各种环境下对ORACLE数据库进行访问。由于该语言集成于数据库服务器中，所以PL/SQL代码可以对数据进行快速高效的处理。 

<!--more-->

## PL/SQL 中可引用的SQL语句 

- 可用DML语句：SELECT INTO，INSERT，UPDATE，DELETE。
- 可用TCL语句：COMMIT，ROLLBACK，SAVEPOINT。
- 不能使用DDL语句。

## PL/SQL 块
PL/SQL块：声明部分+执行部分+异常处理部分

```plsql
-- 单行注释
DECLARE
	/* 声明部分：声明变量，类型、游标、局部存储过程和函数 */
BEGIN
	/* 执行部分：执行过程和SQL语句 */
[EXCEPTION]
	/* 异常处理部分 */
END;
```

## PL/SQL 变量

### PL/SQL 变量命名

| 标识符   | 命名规则        |
| -------- | --------------- |
| 程序变量 | V_name          |
| 程序常量 | C_Name          |
| 游标变量 | Name_cursor     |
| 异常标识 | E_name          |
| 表类型   | Name_table_type |
| 表       | Name_table      |
| 记录类型 | Name_record     |

### PL/SQL 变量类型

#### 基本数据类型

number，char，varchar2，long，date

#### 记录类型

记录类型：把逻辑相关的数据作为一个单元存储起来，用于存放互不相同但逻辑相关的信息。 

```plsql
TYPE record_type IS RECORD(
   Field1 type1  [NOT NULL]  [:= exp1 ],
   Field2 type2  [NOT NULL]  [:= exp2 ],
   . . .   . . .
   Fieldn typen  [NOT NULL]  [:= expn ] ) ;
```

#### %TYPE 类型

%TYPE类型：指某个已定义变量的数据类型类型，或数据表中某列的数据类型。

使用%TYPE特性的优点：

- 所引用的数据库列的数据类型可以不必知道；
- 所引用的数据库列的数据类型可以实时改变。

#### %RowType 类型

%RowType类型：返回一个与数据库表的数据结构一致的记录类型。

使用%ROWTYPE特性的优点：

- 所引用的数据库中列的个数和数据类型可以不必知道；
- 所引用的数据库中列的个数和数据类型可以实时改变。

### PL/SQL 特殊运算符

赋值运算符：`:=`
关系运算符：`=>`
上下限运算符：`..`

## PL/SQL 流程控制

### 条件语句

#### IF 语句

```plsql
IF <条件语句1> THEN
	语句1;
ELSIF <条件语句2> THEN
	语句2;
ELSE
	语句3;
END IF;
```

> 注意是`ELSIF`不是<s>`ELSEIF`</s>；

#### CASE 语句

```plsql
CASE <变量>
	WHEN <值1> THEN <结果1>
	WHEN <值2> THEN <结果1>
	...
	WHEN <值N> THEN <结果N>
	[ELSE <结果N+1>]
END;
```

### 循环语句

#### do...while 循环

```plsql
LOOP
	循环语句;
	EXIT WHEN <条件语句>
END LOOP;
```

#### while 循环

```plsql
WHILE <条件语句> LOOP
	循环语句;
END LOOP;
```

#### for 循环

```plsql
FOR <循环计数器> IN [REVERSE] <下限> .. <上限> LOOP
  循环语句;
END LOOP;
```

### GOTO 语句

定义标号：`<<标号名>>`；

GOTO语句：`GOTO 标号名`；

### NULL 语句

NULL语句：不做任何事，增强代码可读性。

## PL/SQL 异常处理

### 异常错误类型

- 预定义错误：无需在程序中定义，由Oracle自动将其引发。
- 非预定义错误：用户需在程序中定义，然后由Oracle自动将其引发。
- 用户定义错误：用户在程序中定义，然后显式地在程序中将其引发。

### 异常处理

```plsql
EXCEPTION
   WHEN <异常1> THEN  <异常处理代码>
   WHEN <异常2> THEN  <异常处理代码>
   WHEN OTHERS THEN  <异常处理代码>
```


## 游标-CURSOR
为了处理 SQL 语句，Oracle 会分配一片叫上下文 (context area) 的区域来处理所必需的信息，即系统为用户开设的一个数据缓冲区，存放SQL语句的执行结果。游标就是一个指向上下文的句柄或指针。

### 显式游标

显式游标主要是用于对查询语句的处理，尤其是查询结果为多条记录的情况。

#### **显式游标处理**：

①定义游标：定义游标名及对应的 SELECT 查询

```plsql
CURSOR cursor_name (参数1 参数类型,参数2 参数类型...) IS 
SELECT查询块;
```

> 数据类型不能使用长度约束。

②打开游标：执行游标的 SELECT 查询，将查询结果放入缓冲区，并指向缓冲区首部。

```plsql
OPEN cursor_name (参数1 => 值,参数2 => 值...);
```

③提取游标数据：检索结果集中的数据行，放入指定输出变量中。

```plsql
FETCH cursor_name INTO {variable_list | record_variable };
```

④关闭游标：释放游标占用的系统资源。

```plsql
CLOSE cursor_name;
```

#### 显式游标属性

| 游标属性    | 描述                                          |
| ----------- | --------------------------------------------- |
| `%FOUND`    | 布尔型，当最近一次读记录时成功返回,则值为TRUE |
| `%NOTFOUND` | 布尔型，当最近一次读记录时返回失败,则值为TRUE |
| `%ISOPEN`   | 布尔型，当游标已打开时返回 TRUE               |
| `%ROWCOUNT` | 数值型，返回已从游标中读取的记录数            |

#### 游标的FOR循环：

```plsql
FOR 索引 IN 游标[值1,值2...] LOOP
    循环语句;
END LOOP;
```

### 隐式游标

隐式游标主要用于数据更新操作。隐式游标的名字为`SQL`，由Oracle系统提供，无需用户处理。

#### 隐式游标属性

| 游标属性       | 描述                                          |
| -------------- | --------------------------------------------- |
| `SQL%FOUND`    | 布尔型，当最近一次读记录时成功返回,则值为TRUE |
| `SQL%NOTFOUND` | 布尔型，当最近一次读记录时返回失败,则值为TRUE |
| `SQL%ISOPEN`   | 布尔型，当游标已打开时返回 TRUE               |
| `SQL%ROWCOUNT` | 数值型，返回已从游标中读取的记录数            |



## 存储过程与函数
存储过程用于执行特定操作，无返回值；函数用于执行复杂操作，有返回值；存储函数与函数统称为PL/SQL子程序。

### 存储过程-PROCEDURE

存储过程：执行特定操作，无返回值，多用于更新操作。

#### 定义存储过程

```plsql
CREATE [OR REPLACE] PROCEDURE Procedure_name
[ (argment [ { IN | IN OUT }] Type,
      argment [ { IN | OUT | IN OUT } ] Type ]
{ IS | AS }
<类型.变量的说明> 
BEGIN
	<执行部分>
EXCEPTION
	<异常处理>
END;
```

### 函数-FUNCTION

函数：执行复杂操作，有返回值。

#### 定义函数

```plsql
CREATE [OR REPLACE] FUNCTION <函数名>[(argument [ { IN | IN OUT }] type,argument [ { IN | OUT | IN OUT } ] type]RETURN <返回值类型>
{ IS | AS }
	<类型.变量的说明> 
BEGIN
	<执行部分>
EXCEPTION
	<异常处理>
END;
```

#### 函数执行方式

```plsql
dbms_output.put_line(fun());
```

```plsql
select fun() from dual;
```

#### 函数参数类型

- 输入参数 `IN`；
- 输出参数 `OUT`；
- 输入输出参数 `IN OUT`；

#### 参数传递类型

**位置表示法**：根据参数位置依此传值

```plsql
argument_value1[,argument_value2 …]
```

**名称表示法**：使用关系运算符`=>`为参数传值

```plsql
argument => parameter [,…]
```

## 触发器-TRIGGER

触发器：用户定义的一类由事件触发而执行的特殊过程。

### 定义触发器

```plsql
CREATE [OR REPLACE] TRIGGER <触发器名称>
{BEFORE | AFTER | INSTEAD OF}
{INSERT | DELETE | UPDATE [OF column [, column …]]}
ON <表/视图>
[REFERENCING {OLD [AS] old | NEW [AS] new| PARENT as parent}]
[FOR EACH ROW | STATEMENT]
[WHEN condition]
trigger_body;
```

触发事件：INSERT | DELETE | UPDATE

触发时机：BEFORE | AFTER

触发频率：
- ROW：行级触发；当某触发事件发生时，对受到该操作影响的每一行数据，触发器都单独执行一次。
- STATEMENT：语句级触发；当某触发事件发生时，该触发器只执行一次。

触发器的新值与旧值：
- :new  事件触发后的新的数据行；
- :old  事件触发前的旧的数据行；