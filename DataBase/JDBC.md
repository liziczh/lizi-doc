---
title: JDBC
comments: true
date: 2018-05-18 23:33:29
id: jdbc
tags:
- Java
- Database
categories: DataBase
toc: true
reward: true
copyright: true
---

<!--# JDBC-->

JDBC（Java Database Connectivity ，Java数据库连接），一种用于执行SQL语句的Java API，为访问不同的关系型数据库提供了统一规范。

<!--more-->

## JDBC 简介

JDBC 提供了两种规范：

- JDBC API：提供了应用程序与 JDBC 管理器的连接，供**开发人员**连接数据库、执行SQL语句、获得结果。
- JDBC Driver API：提供了 JDBC 管理器与驱动程序的连接，供**数据库厂商**开发数据库驱动程序使用。

数据库厂商通过 JDBC Driver API 开发相应的数据库驱动程序，开发人员只需导入数据库驱动程序，即可使用 JDBC API 编写代码操纵该数据库。

## JDBC API 组件

- **DriverManager**：此接口用于管理一系列数据库驱动程序。匹配连接使用通信子协议从Java应用程序请求相应的数据库驱动程序。识别JDBC在某个子协议的第一个驱动程序将被用来建立数据库连接。 
- Driver：此接口用于处理与数据库服务器的通信。很少直接直接使用驱动程序（Driver）对象，一般使用`DriverManager`中的对象。
- **Connection**：此接口拥有接触数据库的所有方法。连接对象表示通信上下文，即与数据库中的所有的通信是通过此唯一的连接对象。 
- **Statement**：创建该接口对象将SQL语句提交到数据库。
- **ResultSet**：保存使用`Statement`对象执行SQL查询的结果集，可迭代结果集。
- **SQLException**：用于处理发生在数据库应用程序中的错误。

## JDBC 连接数据库

**前提**：安装由数据库厂商提供的数据库驱动程序 (导入数据库驱动jar包)。

**JDBC 连接数据库步骤**：

1. 加载和注册驱动
2. 获取数据库连接
3. 执行sql语句
4. 处理结果集
5. 释放资源

**JDBC 简单示例代码**

```java
public class JDBCDemo {
	public static void main(String[] args) {
		// 声明变量
		Connection conn = null;
		Statement stat = null;
		ResultSet res = null;
		List<User> userList = new ArrayList<>();
        
		try {
			// 1.加载和注册驱动
			String driverClassName = "com.mysql.jdbc.Driver";
			Class.forName(driverClassName);
			// 2.获取数据库连接
			String url = "jdbc:mysql:///lizi";
			String ur = "root";
			String pwd = "root";
			conn = DriverManager.getConnection(url, ur, pwd);
			// 3.执行sql查询语句
			stat = conn.createStatement();
			String sql = " select * from user ";
			res = stat.executeQuery(sql);
			// 4.迭代处理结果集
			while(res.next()) {
				User user = new User();
				user.setId(res.getInt("id"));
				user.setUsername(res.getString("username"));
				user.setPassword(res.getString("password"));
				userList.add(user);
			}
		} catch (ClassNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}finally {
			// 5.关闭连接，释放资源
			try {
				// 释放结果集
				res.close();
			} catch (SQLException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}finally {
				try {
					// 释放statement对象
					stat.close();
				} catch (SQLException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}finally {
					try {
						// 关闭数据库连接
						conn.close();
					} catch (SQLException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}
			}
		}
	}
}
```

### 1.加载和注册驱动

**使用 `DriverManager.registerDriver()` 加载和注册驱动**：

Oracle：

```java
// 注册Oracle驱动
Driver oracleDriver = new oracle.jdbc.driver.OracleDriver();
DriverManager.registerDriver(oracleDriver);
```

MySql：

```java
// 注册MySql驱动
Driver mysqlDriver = new com.mysql.jdbc.Driver();
DriverManager.registerDriver(mysqlDriver);
```

**使用 `Class.forName()` 加载和注册驱动**：

Oracle：

```java
// 加载和注册Oracle驱动
String driverClassName = "oracle.jdbc.driver.OracleDriver";
Class.forName(driverClassName);
```

MySql：

```java
// 加载和注册MySql驱动
String driverClassName = "com.mysql.jdbc.Driver";
Class.forName(driverClassName);
```

> 推荐使用 `Class.forName()` 加载驱动。
>

### 2.获取数据库连接

**（1）使用 `*.properties` 文件配置数据库属性**

Oracle：

```properties
driverClassName=oracle.jdbc.driver.OracleDriver
url=jdbc:oracle:thin:@192.168.124.15:1521:orcl
username=scott
password=tiger
```

MySql：

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/lizi?useUnicode=true&characterEncode=utf8&useSSL=false
username=root
password=root
```

**（2）读取 properties 文件，获取属性值**

`Propertoes` 对象获取属性的方法：

- `load()`：加载文件流。
- `getProperties(String name)`：根据属性名获取属性值。

```java
// 使用properties文件流获取属性值
InputStream in = JDBCUtils.class.getClassLoader().getResourceAsStream(String filepath);
Properties properties = new Properties();
// 加载文件流
properties.load(in);
// 获取properties文件属性值
String url = properties.getProperties("url");
String username = properties.getProperties("username");
String password = properties.getProperties("password");
```

**（3）获取数据库连接**

```java
Connection conn = DriverManager.getConnection(url, username, password);
```

### 3.执行SQL语句

#### Statement 执行SQL语句

```java
// 创建Statement对象执行SQL语句
Statement stat = conn.createStatement();
String sql = " select * from \"user\" ";
// 获取SQL查询结果集
ResultSet res = stat.executeQuery(sql);
```

`Statement` 对象的执行SQL语句方法：

- `boolean execute (String sql)`：如果存在结果集，返回 `true` ；否则返回 `false` 。
- `int executeUpdate (String sql)`：执行SQL更新语句，返回受影响的行数。
- `ResultSet executeQuery(String sql)`： 执行SQL查询语句，返回查询结果集。

#### PreparedStatement 预编译

为了防止SQL注入，采用 PreparedStatement 预编译SQL语句，使用 `?` 作为参数占位符，使用`setXXX(int parameterIndex, XXX x)` 动态填充参数。

```java
// 预编译
String sql = "select * from user where username = ? and password = ? ";
PreparedStatement pstat = conn.prepareStatement(sql);
// 根据'?'索引填充参数值
pstat.setString(1, "admin");
pstat.setString(2, "123456");
// 获取SQL查询结果集
ResultSet res = pstat.executeQuery(sql);
```

- setXXX(int paraIndex, XXX x)：根据 `?` 参数索引 (1,2,3...) 填充XXX类型的数据。

### 4.ResultSet 结果集

`ResultSet`：表示数据库结果集的数据表，通常通过执行**SQL查询**语句生成。

- `boolean next()`：将光标移至下一行。如果没有更多行，返回 `false` 。
- `XXX getXXX(String columnName)`：返回columnName列中当前行的XXX类型值。

```java
// 迭代保存结果集
while(res.next()) {
	User user = new User();
	user.setId(res.getInt("id"));
	user.setUsername(res.getString("username"));
	user.setPassword(res.getString("password"));
	userList.add(user);
}
```

### 5.释放资源

```java
// 释放结果集
res.close();
// 释放statement对象
stat.close();
// 关闭数据库连接
conn.close();
```



## JDBC 事务处理

`Connection` 对象的事务处理方法：

- `setAutoCommit(false)`：关闭自动提交
- `commit()`：提交事务
- `rollback()`：回滚事务
- `setSavepoint(String savepointName)`：设置保存点
- `releaseSavepoint(Savepoint savepointName)`：删除保存点

```java
try{
    // 关闭自动提交事务
    conn.setAutoCommit(false);
    ...
	/* SQL操作 */
	...
	// If there is no error.
    conn.commit();
}catch(SQLException se){
	// If there is any error.
	conn.rollback();
}
```

## JDBC 批量处理

`Statement` 对象的批处理方法：

- `void addBatch()`：添加SQL语句到批处理中。
- `int[] executeBatch()`： 执行所有SQL语句。返回一个整数数组，数组的每个元素表示相应更新语句的更新计数。

```java
// 1.创建Statement对象
Statement stat = conn.createStatement();
// 2.关闭自动提交
conn.setAutoCommit(false);
// 3.添加SQL语句到批处理中
String sql = "INSERT INTO Employees (id, first, last, age) " +
             "VALUES(200,'Ruby', 'Yang', 30)";
stat.addBatch(sql);
......
// 4.执行所有SQL语句，返回更新记数数组
int[] count = stat.executeBatch();
// 5.提交更新
conn.commit();
```

## 数据库连接池

数据库连接池负责分配、管理和释放数据库连接，它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个。释放空闲时间超过最大空闲时间的数据库连接来避免因为没有释放数据库连接而引起的数据库连接遗漏。

数据库连接提前创建了多个数据库连接，避免在程序运行期间频繁打开/关闭数据库连接造成的性能损耗。这项技术能明显提高对数据库操作的性能。 

总而言之，数据库连接是一种昂贵的资源，采用数据库连接池可以控制连接数量，复用连接，提高性能。

### 常用的数据库连接池

- **DBCP**，Apache 提供的一个Java连接池项目，配置简单，没有连接池监控功能，大并发下速度稍慢。

```java
BasicDataSource ds = new BasicDataSource();
```

- **C3P0**，配置简单，没有连接池监控功能，大并发下速度稳定。

```java
ComboPooledDataSource ds = new ComboPooledDataSource();
```

- **Druid**，阿里巴巴开源提供的一个数据库连接池实现，在DBCP、C3P0等的基础上添加了日志监控功能。

```java
DataSource dataSource = DruidDataSourceFactory.createDataSource();
```

### 数据库连接池的使用

以DBCP为例，其他大同小异。

**前提**：安装数据库连接池程序（导入dbcp的jar包）

**（1）dbcp.properties**

```properties
#基本配置
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/lizi
username=root
password=root
#初始化池大小
initialSize=10
#最大连接数
maxActive=8
#最大空闲连接
maxIdle=8
#最小空闲连接
minIdle=0
#最大等待时间
maxWait=-1
#连接属性
connectionProperties=useUnicode=true;characterEncoding=UTF8;useServerPrepStmts=true;cachePrepStmts=true;prepStmtCacheSize=50;prepStmtCacheSqlLimit=300
#连接的默认提交方式
defaultAutoCommit=true
#连接是否为只读连接
defaultReadOnly=false
#指定事务的事务隔离级别
defaultTransactionIsolation=REPEATABLE_READ
```

**（2）使用 DataSource 获取数据库连接**

```java
// 创建 DataSource 数据源对象
BasicDataSource ds = new BasicDataSource();
// 设置数据库连接池参数
ds.setDriverClassName(properties.getProperty("driverClassName"));
ds.setUrl(properties.getProperty("url"));
ds.setUsername(properties.getProperty("username"));
ds.setPassword(properties.getProperty("password"));
ds.setMaxActive(Integer.parseInt(properties.getProperty("maxIdle")));
ds.setMaxWait(Long.parseLong(properties.getProperty("maxWait")));
ds.setMaxActive(Integer.parseInt(properties.getProperty("maxActive")));
ds.setInitialSize(Integer.parseInt(properties.getProperty("initialSize"));
// 获取数据库连接
Connection conn = ds.getConnection();
```

## DBUtils

Commons DBUtils 是 Apache 提供的一个对 JDBC 进行简单封装的开源工具类库。

### DBUtils 组件

- **QueryRunner**：执行SQL语句。
- **ResultSetHandle**：封装结果集的策略对象（将数据存入对象、数组、集合等）。

### DBUtils 使用

**前提**：前往 Apache 官网下载安装DBUtils（导入dbutils的jar包）。

`QueryRunner` 对象执行SQL语句的方法：

- `<T> T query(String sql,ResultSetHandler<T> rsh,Object... params)`：执行SQL查询语句，返回结果对象/数组/集合。
- `int update(String sql,Object...params)`：执行SQL更新。

**（1）创建 QueryRunner 对象**

```java
QueryRunner qr = new QueryRunner(DataSource ds);
```

**（2）执行SQL查询语句（有返回值）**

```java
String sql = "select * from \"product\" where \"proId\" = ?";
Product product = qr.query(sql,new BeanHandler<>(Product.class),id);
```

**（3）执行SQL更新语句（无返回值）**

```java
String sql = "delete from \"product\" where \"proId\" = ?;
qr.update(sql,id);
```

