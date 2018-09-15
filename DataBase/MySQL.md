# MySQL

Windows启动/关闭MySQL：

```powershell
net start mysql # 开启mysql服务
net stop  mysql # 关闭mysql服务
```

Linux启动/关闭MySQL：

```shell
service mysql start | stop | restart | status
```

登录MySQL数据库：

```shell
mysql -u <用户名> -p <密码>
```

更改用户密码：

```mysql
mysqladmin -u <用户名> password <新密码>
```

## DDL

### 数据库

创建数据库：

```mysql
CREATE DATABASE <数据库名> [CHARACTER SET utf8];
```

删除数据库：

```mysql
DROP DATABASE <数据库名>;
```

查看：

查看所有数据库：

```mysql
show databases;
```

选择数据库：

```mysql
use <数据库名>;
```

查看当前库：

```mysql
select database();
```

查看当前库的创建信息：

```mysql
show create database <数据库名>;
```

查看当前库中所有表：

```mysql
show tables;
```



### 表

创建表：

```mysql
CREATE TABLE <表名> (
	<列名> <数据类型> [<约束>],
    <列名> <数据类型> [<约束>]
);
```

删除表：

```mysql
DROP TABLE <表名>;
```

查看：

查看表结构：

```mysql
desc <表名>;
```

查看表的创建信息：

```mysql
show create table <表名>;
```

修改：

重命名表：

```mysql
RENAME TABLE <旧表名> TO <新表名>;
```

添加字段：

```mysql
ALTER TABLE <表名> ADD <新增列名> <数据类型>;
```

删除字段：

```mysql
ALTER TABLE <表名> DROP <被删除的列名>;
```

修改字段：

```mysql
ALTER TABLE <表名> CHANGE <旧列名> <新列名> <数据类型>;
```

### 数据分页

```
limit <起始下标>,<每页数据>
```

### 数据库备份与还原

备份：

```shell
mysqldump -u<用户名> -p<密码> 需备份的数据库名 > 备份后的SQL文件名;
```

还原：创建一个新数据库，在新数据库中还原；

```shell
source <备份SQL文件路径>; 
```



