### Spring Core（一）IOC容器

控制反转（IoC），依赖注入（DI）。

Spring IoC容器框架的基础：org.springframework.beans和org.springframework.context。

BeanFactory 接口提供了配置框架的基本功能，ApplicationContext是BeanFactory的一个子接口，添加了配置企业应用的更多功能。

#### 容器概览

ApplicationContext是Spring IoC容器实现的代表，负责实例化、配置和装配实现bean的接口。

POJOS + 配置元数据（XML/注解） --> SpringContainer。

- 基于XML配置元数据：

- 基于注解配置元数据：@Bean。

基于XML配置元数据：

```xml
<beans>
    <!--导入外部文件-->
    <import resource="xxx.xml" />
    <!--Bean定义-->
	<bean id="bean1" class="...">
    	<property name="prop1" ref=""></property>
     	...
    </bean>
    <bean id="bean2" class="..."></bean>
    <!--more bean definitions--> 
</beans>
```

使用容器：ApplicationContext可以读取并访问bean。

```java
// 读取XML配置文件
ApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"service.xml","daos.xml"});
// 获取Bean
PetStoreService service = context.getBean("petStore", PetStoreService.class);
```

#### Bean概览

在容器内部，bean被定义为BeanDefinition对象。

bean定义属性：

- id - Bean 命名
- name - 名称
- class - 类
- scope - 作用域
- constructor arguments - 构造注入
- properties - 属性注入
- autowiring mode - 自动装配
- lazy-initialization mode - 延迟初始化
- initialization method - 初始化方法回调
- destruction method - 销毁方法回调

定义 Bean 别名：

```xml
<alias name="fromName" alias="toName" />
```

实例化Bean：

a）直接根据 class 属性反射调用构造方法创建 Bean。

```xml
<bean id="exampleBean" class="example.exampleBean"></bean>
```

b）静态工厂方法创建，类中包含静态方法，通过调用静态方法返回对象的类型可能与Class一样，也可能不一样。

```xml
<bean id="exampleBean" class="example.exampleBean" factory-method="createInstance"></bean>
```

c）实例工厂方法实例化

```xml
<bean id="exampleBean" factory-bean="serviceLocator" factory-method="createInstance"></bean>
```

#### 依赖注入

基于**构造函数**的依赖注入：

a）使用type属性指定构造参数类型：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<constructor-arg type="int" value="123"></constructor-arg>
    <constructor-arg type="java.lang.String" value="Chen"></constructor-arg>
</bean>
```

b）使用 index 属性指定构造参数位置：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<constructor-arg index="0" value="123"></constructor-arg>
    <constructor-arg index="1" value="Chen"></constructor-arg>
</bean>
```

c）使用 name 属性匹配参数名称：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<constructor-arg name="height" value="123"></constructor-arg>
    <constructor-arg name="firstName" value="Chen"></constructor-arg>
</bean>
```

> @ConstructorProperties({height}, {firstName})：显示声明构造函数名称。

基于**setter方法**的依赖注入：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<property name="prop1" value="123"></property>
    <property name="prop2" ref="bean2"></property>
</bean>
```

其他注入：

a）内部类注入：内部类不需要指定 id 或者 name。

```xml
<bean id="exampleBean" class="example.exampleBean">
	<property name="target">
		<bean class="com.example.Person">
            <property name="name" value="Tom"></property>
    		<property name="age" ref="25"></property>
        </bean>
	</property>
</bean>
```

b）集合注入：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<property name="list">
		<value>苹果</value>
        <ref bean="bean1"/>
	</property>
    <property name="set">
		<value>苹果</value>
        <ref bean="bean1"/>
	</property>
    <property name="map">
		<map>
        	<entry key="k1" value="v1"/>
            <entry key="k1" ref="bean2"/>
        </map>
	</property>
</bean>
```

c）properties 注入：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<property name="adminEmails">
		<props>
        	<prop key="123">123@example.com</prop>
            <prop key="support">support@example.com</prop>
        </props>
	</property>
</bean>
```

d）null 值注入：

```xml
<bean id="exampleBean" class="example.exampleBean">
	<property name="email">
		<null/>
	</property>
</bean>
```

