# MySQL

存储引擎：
MyISAM引擎：查询性能高，不支持事务、外键。
innoDB引擎：支持事务、外键。

注释：`##`

数据类型
char：定长字符串
varchar：变长字符串
选择：
1.整形：
2.浮点型：
3.字符串：char定长，varchar变长
4.时间：一般用字符串，特殊情况用TIMESTAMP
5.ENUM和SET：
6.TEXT和BLOB：TEXT文本，多媒体文件BLOB


转换NULL值：IFNULL(字段,替换值)；
CONCAT()：字符串拼接；


分页设计：
假分页（逻辑分页）：将数据全部查出，存于内存，直接从内存取。
真分页（物理分页）：数据库中分页。

`LIMIT currentPage+1 pageSize`
> currentPage：从第几条开始;  
> pageSize：每页几条;
> (currentPage-1)*pageSize;

```
select * from product 
LIMIT 0 5;
```

登陆数据库
mysql

数据库备份：

```
mysql dump -uroot -proot 数据库名>路径  ## 备份数据库
mysql -uroot -proot 数据库名<备份数据库路径  ## 导入备份数据库
```



事务：innoDB引擎
开启事务：start transaction


