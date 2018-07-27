---
title: Oracle新建数据库
comments: true
date: 2018-05-20 14:56:29
id: oracle-newdb
tags:
- Oracle
- Database
categories: DataBase
toc: true
reward: true
copyright: true
---

<!--# Oracle新建数据库-->

Oracle新建数据库，即新建一块表空间，将表空间分配给某个用户。

<!--more-->

#### **1.创建表空间**：

```mysql
CREATE TABLESPACE <表空间名> 
DATAFILE 'C:\app\oradata\orcl\date.dbf' 
SIZE <空间>[K|M] 
[AUTOEXTEND [OFF|ON]];
```

#### **2.创建用户**：

```mysql
CREATE USER <用户名> IDENTIFIED BY <密码>  
DEFAULT TABLESPACE <表空间名> 
[TEMPORARY TABLESPACE <l临时表空间名>];
```

#### **3.将表空间分配给用户**：

```mysql
ALTER USER <用户名> DEFAULT TABLESPACE <表空间名>;
```

#### **4.给用户授权**：

```mysql
GRANT create session,create table,unlimited tablespace 
TO <用户名>;
```

#### **5.用户登陆**：

```mysql
conn <用户名>/<密码>;
```