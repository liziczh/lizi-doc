# Spring

Spring IOC（控制反转） 和 AOP（面向切面编程）。

IOC：

- 依赖查询（DL）
- 依赖注入（DI）

AOP：面向切面编程，横向

## Spring 与 IOC

IOC（Inversion of Control，控制反转）

### 引入依赖

- 普通 Project 引入 jar 包：`spring-core`，`spring-context`，`spring-beans`，`spring-expression`，`commons-loging`，log4j，junit。
- Maven Project 引入依赖：spring-context，commons-loging（日志规范），log4j（日志实现），junit。

Maven **pom.xml**：

```xml
<dependencies>
    <!--引入spring依赖-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
    <!--引入日志相关-->
    <!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
    <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>1.2</version>
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <!--引入单元测试-->
    <!-- https://mvnrepository.com/artifact/junit/junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

**log4j.properties**：

```properties
log4j.rootCategory=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %t %c{2}:%L - %m%n
log4j.category.org.springframework.beans.factory=DEBUG
```

### Spring 配置文件

**spring-cfg.xml**：

多个配置文件：一个主配置文件，多个从配置文件；

```xml
<import resource="spring-*-cfg.xml"></import>
```

### Bean 装配

#### 默认装配

使用无参构造。

```xml
<bean id="student" class="com.lizi.pojo.Student" scope="singleton"></bean>
```

#### 动态工厂

**spring-config.xml**：指定 `factory-bean` 和 `factory-method`；

```xml
<bean id="studentFac" class="com.lizi.pojo.StudentFactory"></bean>
<bean id="student" factory-bean="studentFac" factory-method="getStudent" ></bean>
```

#### 静态工厂

**spring-config.xml**：指定 `class` 和 `factory-method`；

```xml
<bean id="student" class="com.lizi.pojo.StudentFactory" factory-method="getStudent"></bean>
```

#### Bean 作用域

**scope**：

1. singleton：单例模式。即在整个Spring容器中，使用singleton定义的Bean将是单例的，只有一个实例。默认为单态的。
2. prototype：原型模式。即每次使用getBean方法获取的同一个的实例都是一个新的实例。
3. request：对于每次HTTP请求，都将会产生一个不同的Bean实例。
4. session：对于每个不同的HTTP session，都将产生一个不同的Bean实例。
5. global session：每个全局的HTTP session对应一个Bean实例。典型情况下，仅在使用portlet 集群时有效，多个Web应用共享一个session。一般应用中，global-session与session是等同的。

### 基于 XML 的 DI

> - `<bean>`：Java Bean
>   - id：Bean 的唯一标识；
>   - name：Bean 的唯一名称，不可与 id 相同；
>   - class：Bean 的全类名；
>   - scope：Bean 的作用域；
>   - factory-bean：工厂 Bean；
>   - factory-method：调用静态工厂方法创建实例；
>   - init-method：Bean 初始化方法；
>   - destory-method：Bean 销毁方法；
>   - autowire：自动装配方式；
>     - byName：根据属性名自动装配
>     - byType：如果容器中存在一个与指定属性类型相同的bean，则自动装配；如果存在多个相同的bean，则抛出异常；如果不存在，则什么也不发生。
> - `<property>`：属性
> - `<constructor-arg>`：构造参数
>   - name：属性名
>   - value：属性值
>   - ref：属性引用

#### 属性注入

属性注入：使用 setter() 注入；

```xml
<!--属性注入：使用 setter() 注入；-->
<bean id="student" class="org.lanqiao.pojo.Student" scope="prototype">
    <property name="sid" value="1"></property>
    <property name="sname" value="张三"></property>
    <property name="age" value="21"></property>
    <property name="school" ref="school"></property>
</bean>
<bean id="school" class="org.lanqiao.pojo.School">
    <property name="schoolName" value="太原师范学院"></property>
</bean>
```

#### 构造注入

构造注入：使用 constructor 注入；

```xml
<!--构造注入：使用 constructor 注入；-->
<bean id="student2" class="com.lizi.pojo.Student" >
    <constructor-arg name="id" value="2"></constructor-arg>
    <constructor-arg name="name" value="zhangsna"></constructor-arg>
    <constructor-arg name="sex" value="男"></constructor-arg>
    <constructor-arg name="age" value="12"></constructor-arg>
    <constructor-arg name="school" ref="school2"></constructor-arg>
</bean>
<bean id="school2" class="com.lizi.pojo.School">
    <constructor-arg name="id" value="1"></constructor-arg>
    <constructor-arg name="schoolName" value="太原师范学院"></constructor-arg>
</bean>
```
#### 集合注入

```xml
<!--集合注入：List，Set，Map，Array-->
<property name="courses">
    <list>
        <value>语文</value>
        <value>数学</value>
        <value>英语</value>
    </list>
</property>
<property name="teachers">
    <set>
        <value>陈老师</value>
        <value>张老师</value>
        <value>王老师</value>
    </set>
</property>
<property name="scores">
    <map>
         <entry key="语文" value="59"></entry>
         <entry key="数学" value="89"></entry>
         <entry key="英语" value="120"></entry>
    </map>
</property>
```

#### 命名空间注入

**p 命名空间注入**：属性注入（p=property）；

```xml
<!--xml 文件头部设置-->
xmlns:p="http://www.springframework.org/schema/p"
```

```xml
<bean id="p-namespace" class="com.lizi.pojo.School" p:id="12" p:schoolName="TNU" />
```

**c 命名空间注入**：构造注入（c=constructor）;

```xml
<!--xml 文件头部设置-->
xmlns:c="http://www.springframework.org/schema/c"
```

```xml
<bean id="c-namespace" class="com.lizi.pojo.School" c:id="12" c:schoolName="TNU" />
```

#### 自动装配-autowire

`byName`：根据属性名自动装配；

```xml
<bean id="student" name="student" class="com.lizi.pojo.Student" autowire="byName">
   <property name="id" value="1"></property>
   <property name="name" value="Tom"></property>
</bean>
```

`byType`：如果容器中存在一个与指定属性类型相同的bean，则自动装配；如果存在多个相同的bean，则抛出异常；如果不存在，则什么也不发生；

```xml
<bean id="student" name="student" class="com.lizi.pojo.Student" autowire="byType">
   <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
        <property name="sex" value="男"></property>
        <property name="age" value="12"></property>
        <property name="school" ref="school1"></property>
</bean>
```

#### SpEL

```xml
<bean id="student4" class="com.lizi.pojo.Student" autowire="byName">
    <property name="id" value="1"></property>
    <property name="name" value="#{'tom'.toUpperCase()"></property>
    <property name="sex" value="男"></property>
    <property name="age" value="#{T(Math).random()*100}"></property>
</bean>
```

#### 内部 Bean

```xml
<bean id="student4" class="com.lizi.pojo.Student" autowire="byName">
    <property name="id" value="1"></property>
    <property name="name" value="张三"></property>
    <property name="school">
        <bean class="com.lizi.pojo.School">
            <property name="schoolName" value="TNT"></property>
        </bean>
     </property>
</bean>
```

### 基于注解的 DI

`@Component` - 标注当前类为 Spring 的一个组件；

`@Respository` - 标注RespositoryImpl/DaoImpl；

`@Service` - 标注Service；

`@Controller` - 标注Controller/Action；

`@Scope` - 指定 Bean 作用域；默认singleton

`@Autowired` - 标注自动装配方式，默认byType；

`@Qualifier` - 指定自动装载的bean的name；

`@Value` - 基本类型属性注入；

`@Resource` - 域属性注解；

**spring-*-cfg.xml**：

（1）引入 context 命名空间；

```xml
<!--xml 头部-->
<beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:context="http://www.springframework.org/schema/context"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
```

（2）引入组件自动扫描

```xml
<!--组件自动扫描-->
<context:component-scan base-package="com.lizi.pojo"></context:component-scan>
```

#### Bean 的定义：@Component

`@Component` - 标注当前类为 Spring 的一个组件；

`@Respository` - 标注RespositoryImpl/DaoImpl；

`@Service` - 标注Service；

`@Controller` - 标注Controller/Action；

#### Bean 的作用域：@Scope

`@Scope` - 指定 Bean 作用域；默认 `singleton`；

```
@Scope("prototype")
```

#### 按类型自动装配：@Autowired

```
@Autowired
```

#### 按名称自动装配：@Autowired和@Qualifier

```
@Autowired
@Qualifier("myStudent")
```


#### 基本类型属性注入：@Value

`@Value` - 基本类型属性注入；可用于属性&setter()方法上。

```xml
@Value("1")
```

#### 测试注解

```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:spring-annotation-cfg.xml"})
```

## Spring 与 AOP

AOP（Aspect Orient Programming，面向切面编程）。

AOP 底层是使用动态代理模式，将交叉业务逻辑织入到主业务逻辑中。

### AOP 术语

（1）切面（Aspect）：泛指交叉业务逻辑。比如事务处理、日志处理等。常用的切面有通知与顾问，用于增强主业务逻辑。

（2）织入（Weaving）：指将交叉业务逻辑插入主业务逻辑的过程。

（3）连接点（JoinPoint）：指被切面织入的方法，通常业务接口中的方法均为连接点。

（4）切入点（Pointcut）：指切面具体织入的方法。

（5）目标对象（Target）：指将要被增强的对象。

（6）通知（Advice）：通知是切面的一种实现，完成简单织入功能。通知定义了增强代码切入到目标代码的时间点，是目标方法执行之前执行，还是之后执行等。通知类型不同，切入时间不同。 切入点定义切入的位置，通知定义切入的时间。 

（7）顾问（Advisor）：顾问是切面的另一种实现，能够将通知以更为复杂的方式织入到目标对象中，是将通知包装为更复杂切面的装配器。 

### Spring AOP 规则

Spring代理规则：

1、默认使用Java动态代理来创建AOP代理，这样就可以为任何接口实例创建代理了

2、当需要代理的类不是代理接口的时候，Spring会切换为使用CGLIB代理，也可强制使用CGLIB

AOP编程：

1、定义普通业务组件

2、定义切入点，一个切入点可能横切多个业务组件

3、定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作

### 引入依赖

Maven **pom.xml**：

```xml
    <dependencies>
        <!--引入spring依赖-->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.18.RELEASE</version>
        </dependency>
        <!--引入日志相关-->
        <!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <!--引入单元测试-->
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!--spring 切面编程-->
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>4.3.18.RELEASE</version>
        </dependency>
    </dependencies>
```

**spring-*-cfg.xml**：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:aop="http://www.springframework.org/schema/aop"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
```

### AspectJ 五种通知类型

AspectJ中常用的通知有五种类型：

`@Before`：前置通知, 在方法执行之前执行

`@After`： ：后置通知（最终通知）, 在方法执行之后执行

`@After-Running`：返回通知, 在方法返回结果之后执行

`@After-Throwing`：异常通知, 在方法抛出异常之后

`@Around`：环绕通知, 围绕着方法执行

### AspectJ 切入点表达式

```xml
execution (
[modifiers-pattern] 访问权限类型 
ret-type-pattern 返回值类型 
[declaring-type-pattern] 全限定性类名 
name-pattern(param-pattern) 方法名(参数名)
throws-pattern] 抛出异常类型 
) 
```








