# MyBatis

MyBatis 是一种支持定制化 SQL、存储过程以及高级映射的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以对配置和原生Map使用简单的 XML 或注解，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。 

## ORM

ORM（Object Relational Mapping，对象关系映射），一个持久化类对应一张数据表，持久化类的每一个实例对应数据表中的一条记录。

## MyBatis 入门

1 引入依赖

```

```

## MyBatis 配置文件

MyBatis 映射配置文件：`mybatis-config.xml` ；

### properties

**properties** 属性文件：外部配置，可动态改变。

1.引入 resources 文件夹下的 *.properties 文件 ：

```xml
<properties resource="com/lizi/test/config.properties" />
```

2.`${xxx}` 动态引用 `*.properties` 文件中 xxx 的属性值：

```xml
<dataSource type="POOLED">
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <property name="username" value="${username}"/>
  <property name="password" value="${password}"/>
</dataSource>
```

### mappers

mappers 映射文件

1.使用 **resource 映射文件路径** 配置映射文件

```xml
<mappers>
    <mapper resource="com/lizi/dao/UserMapper.xml"/>
<mappers>
```

2.使用 **class 接口全类名** 配置映射文件

```xml
<mappers>
    <mapper class="com.lizi.dao.UserMapper"></mapper
<mappers>
```

> 使用条件：
>
> - 映射文件名与Dao接口名称必须相同；
> - 映射文件与接口必须在同一个包下；
> - 映射文件中的的namespace属性值为dao接口的全类名 也可以使用别名；

3.使用 **name 包名** 配置映射文件

```xml
<mappers>
    <package name="com/lizi/dao"></package>
<mappers>
```

> 使用条件：
>
> - dao 使用 mapper 动态代理机制实现；
> - 映射文件名与 dao 接口名称必须相同；
> - 映射文件与接口必须在同一个包下；
> - 映射文件中的的 namespace 属性值为dao接口的全类名；

## MyBatis SQL 映射文件



`#{}`：代表占位符 `?` ；防止SQL注入。

`${}`：代表 `getter()` ；参数传递，字符串拼接。

## MyBatis 引入日志系统 log4j

#### 引入依赖

```xml
<!--添加log4j日志框架-->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

#### 日志级别

 为了方便对于日制信息的输出显示，对日志内容进行了分级管理。共分为6个级别：fatal(致命的)、error、warn、info、debug、trace(堆栈) 。

Log4j 建议只使用四个级别：ERROR、WARN、INFO、DEBUG。

#### 日志的输出控制文件

Log4j 的日志输出控制文件用于设置日志信息内容及其级别，主要由三部分组成： 

- 日志信息的输出位置：控制日志信息将要输出的位置，是控制台还是文件等。 
- 日志信息的输出格式：控制日志信息的显示格式，即怎样的字符串形式显示。
- 日志信息的输出级别：控制日志信息的显示内容，即显示那些级别的日志信息。 

**log4j.properties**：

```properties
### 设置###
log4j.rootLogger = debug,stdout,D,E

### 输出信息到控制抬 ###
log4j.appender.stdout = org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target = System.out
log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern = [%-5p] %d{yyyy-MM-dd HH:mm:ss,SSS} method:%l%n%m%n

### 输出DEBUG 级别以上的日志到=E://logs/error.log ###
log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
log4j.appender.D.File = E://logs/log.log
log4j.appender.D.Append = true
log4j.appender.D.Threshold = DEBUG
log4j.appender.D.layout = org.apache.log4j.PatternLayout
log4j.appender.D.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n

### 输出ERROR 级别以上的日志到=E://logs/error.log ###
log4j.appender.E = org.apache.log4j.DailyRollingFileAppender
log4j.appender.E.File =E://logs/error.log
log4j.appender.E.Append = true
log4j.appender.E.Threshold = ERROR
log4j.appender.E.layout = org.apache.log4j.PatternLayout
log4j.appender.E.layout.ConversionPattern = %-d{yyyy-MM-dd HH:mm:ss}  [ %t:%r ] - [ %p ]  %m%n
```

#### 日志的实现

创建日志对象 Logger，通过Logger的方法在代码中加入日志输出语句。在Java代码中输出日志，需要用到 Logger 类的静态方法 `getLogger()`；

## MyBatis API



## MyBatis 动态代理机制

Mapper 动态代理规范：

- Mapper.xml文件中的namespace与Mapper接口的类路径相同。 
- Mapper接口方法名和Mapper.xml中定义的每个statement的id相同。
- Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同。
-  Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同。



## MyBatis CURD

插入

更新

删除

查询所有

查询单个数据

模糊查询

多参数条件查询

动态SQL

