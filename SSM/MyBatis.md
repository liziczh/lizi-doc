# MyBatis

MyBatis 是一种支持定制化 SQL、存储过程以及高级映射的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以对配置和原生Map使用简单的 XML 或注解，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。 

## ORM

ORM（Object Relational Mapping，对象关系映射），一个持久化类对应一张数据表，持久化类的每一个实例对应数据表中的一条记录。

## MyBatis 入门

1 引入依赖

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.4</version>
</dependency>
<dependency>
     <groupId>mysql</groupId>
     <artifactId>mysql-connector-java</artifactId>
     <version>5.1.38</version>
</dependency>
```

## MyBatis 配置文件

MyBatis 映射配置文件：`mybatis-config.xml` ；

```xml
<?xml version="1.0" encoding="UTF-8" ?>
        <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--jdbc.properties-->
    <properties resource="jdbc.properties"/>
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <!--设置别名-->
    <typeAliases>
        <!--使用pakcage指定别名，包下所有类都可用简单类名作为别名-->
        <package name="com.lizi.pojo"></package>
    </typeAliases>
    <!--项目环境-->
    <environments default="development">
        <!--环境一：development-->
        <environment id="development">
            <!--事务管理器-->
            <transactionManager type="JDBC"/>
            <!--数据源-->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--映射文件-->
    <mappers>
        <package name="com/lizi/dao"></package>
    </mappers>
</configuration>
```

### properties

**properties** 属性文件：外部配置，动态引入。

1.引入 resources 文件夹下的 *.properties 文件 ：

```xml
<properties resource="jdbc.properties" />
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

### settings

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

### typeAliases

为 JavaBean 配置别名：

```xml
<typeAliases>
    <typeAlias type="com.lizi.pojo.User" alias="user"/>
</typeAliases>
```

为 package 配置别名：package 下所有类均可使用简单类名作为其别名。

```xml
<typeAliases>
    <!--使用pakcage指定别名，包下所有类都可用简单类名作为别名-->
    <package name="com.lizi.pojo"></package>
</typeAliases>
```

### environments

```xml
<environments default="development">
    <!--环境一：development-->
    <environment id="development">
        <!--配置事务管理器-->
        <transactionManager type="JDBC"/>
        <!--配置数据源-->
        <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
         </dataSource>
    </environment>
    <!--环境二：development2-->
    <environment id="development2">
        ......
    </environment>
</environments>
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

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lizi.dao.UserMapper">
    <!--SQL查询语句-->
    <select id="getUserById" parameterType="int" resultType="User">
        select * from user where uid = #{id}
    </select>
    <!--SQL查询语句-->
    <select id="getAllUser" resultType="User">
        select * from user
    </select>
    <!--SQL插入语句-->
    <insert id="addUser" parameterType="User">
        insert  into user values(#{id},#{username},#{password})
    </insert>
    <!--SQL更新语句-->
    <update id="updateUserByUid" parameterType="User">
        update user set username = #{username}, password = #{password} where uid = #{id}
    </update>
    <!--SQL删除语句-->
    <delete id="deleteUserByUid" parameterType="int">
        delete from user where uid = #{id}
    </delete>

    <!--SQL查询 返回Map-->
    <select id="getAllUserMap" resultType="User">
        select * from user
    </select>

</mapper>
```

**namespace**：命名空间，一般为包名+ SQL 映射文件名。





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

## MyBatis 使用流程

```java
// 1.加载配置文件：流加载
InputStream in = Resources.getResourceAsStream("mybatis-config.xml");
// 2.创建SqlSession对象：SqlSessionFactory.openSession();
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(in);
SqlSession sqlSession = sqlSessionFactory.openSession();
// 3.sqlSession执行SQL语句
sqlSession.select();
sqlSession.insert();
sqlSession.update();
sqlSession.delete();
sqlSession.commit();
// 4.关闭sqlSession
if(sqlSession!=null){
    sqlSession.close();
}
```



## MyBatis 动态代理机制

Mapper 动态代理规范：

- Mapper.xml文件中的**namespace**与**Mapper接口的类路径**相同。 
- Mapper**接口方法名**和Mapper.xml中定义的每个**statement**的**id**相同。
- Mapper接口方法的**输入参数类型**和mapper.xml中定义的每个sql 的**parameterType**的类型相同。
-  Mapper接口方法的**输出参数类型**和mapper.xml中定义的每个sql的**resultType**的类型相同。



## MyBatis CURD

插入

更新

删除

查询所有

查询单个数据

模糊查询

多参数条件查询

动态SQL

## 一对多





## 多对一





## 多对多