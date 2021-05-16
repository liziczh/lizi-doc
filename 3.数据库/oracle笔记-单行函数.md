# oracle基础

身份：
SYS/system：超级管理员
SYSTEM/manager：
scott/tiger：
SH/sh：


锁定用户hr：

```sql
alter user hr account lock;
```

解锁用户hr：

```sql
alter user hr account unlock;
```

设置密码：

```sql
alter user hr identified by hr;
```

SQLplus格式化操作：
调整每行显示长度：SETLINESIZE 300;
调整每页显示行数：SETPAGESIZE 
每列宽度：COL 列名 FOR A宽度

SQLplus登录：
CONN 账号/密码 as 身份
showuser：显示用户

### 字符串函数

1.大小写转换

```sql
UPPER(ch)
LOWER(ch)
```

2.字符串替换

```sql
REPLEACE(字符串/字段, 被替换内容, 替换内容)
```

3.截取

```
SUBSTR(字符串/字段，截取开始索引，截取字符个数)；索引从1开始。
```

4.去两端空格

```sql
TRIM(字符串/字段)
```

### 数值函数

1.四舍五入

```sql
ROUND(date, [保留小数位])
```

2.截取小数函数

```sql
TRUNC(date, [保留小数位])
```

3.求模取余

```
MOD(数字1/列1, 数字2/列2)
```

### 日期函数

oracle中提供了两个伪列：SYSDATE，SYSTIMESTAMP；

MONTH_BETWEEN()

TO_DATE(ch, format)









