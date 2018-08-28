# SpringMVC

SpringMVC 表现层框架。

## 引入相关依赖

```xml
<dependencies>
    <!--引入spring依赖-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
    <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.18.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-context-support -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context-support</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
    <!--Spring Tx-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
    <!--JSTL-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
    <!--taglibs-->
    <dependency>
        <groupId>taglibs</groupId>
        <artifactId>standard</artifactId>
        <version>1.1.2</version>
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
    <!--引入spring的测试包-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>4.3.18.RELEASE</version>
        <scope>test</scope>
    </dependency>
    <!--引入单元测试-->
    <!-- https://mvnrepository.com/artifact/junit/junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!--Spring JDBC-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>4.3.18.RELEASE</version>
    </dependency>
</dependencies>
```

## SpringMVC 概述

### **SpringMVC 工作流程**

1. 客户端提交请求到 DispatcherServlet（中央调度器）。
2. DispatcherServlet 查询 HandlerMapping（处理器映射器），找到处理请求的 Controller（处理器）。
3. DispatcherServlet 将请求转发给 Controller，Controller 处理请求，返回 ModelAndView（实体与视图）。
4. DispatcherServlet 查询 ViewResolver（视图解析器），找到 ModelAndView 指定的视图，渲染显示到客户端。

### SpringMVC 组件

- **DispatcherServlet**（中央调度器）：负责调度其他组件处理用户请求。
- **HandlerMapping**（处理器映射器）：负责根据用户请求 URL 找到相应的 Handler。
- **HandlAdapter**（处理器映射器）：负责适配多种类型的处理器。
- **Handler**（处理器）：负责处理请求。
- **ViewResolver**（视图解析器）：负责将处理结果的逻辑视图名解析为具体视图。

| 组件名称     | 默认值                                                       |
| ------------ | ------------------------------------------------------------ |
| 处理器映射器 | BeanNameUrlHandlerMapping<br/>DefaultAnnotationHandlerMapping |
| 处理器适配器 | HttpRequestHandlerAdapter<br>SimpleControllerHandlerAdapter<br>AnnotationMethodHandlerAdapter |
| 视图解析器   | InternalResourceViewResolver                                 |

### DispatcherServlet 中央调度器 

DispatcherServlet 中央调度器。

**DispatcherServlet 主要职责**：**调度** 

- 通过 HandlerMapping（处理器映射器）将请求映射到处理器（返回一个 HandlerExecutionChain，包括一个 Controller 处理器、多个 HandlerInterceptor 拦截器）。
- 通过 HandlerAdapter（处理器适配器）适配多种类型的处理器（HandlerExecutionChain中的处理器）。
- 通过 ViewResolver（视图解析器）将逻辑视图名解析到具体视图的实现。

#### DispatcherServlet 配置

**注册 DispatcherServlet** ：

**web.xml**：

```xml
<!--dispatcherServlet 是 SpringMVC 中央调度器-->
<servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--SpringMVC配置文件的位置-->
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <!--启动容器时，初始化该Servlet-->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <!--只拦截*.do请求-->
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

> 在启动 web 容器时，web 项目就初始化 DispatcherServlet。

**url-pattern**：

1.后缀匹配方式：`*.do` 或 `*.action` ，建议使用。

2.不能写为 `/*`：因为 DispatcherServlet 会将跳转动态页面 ( JSP ) 的请求当作一个 Controller 请求，为其查询相应的处理器，当然是找不到的。

3.不能写为`/`：因为 DispatcherServlet 会将获取静态资源 ( .js，.css，.jpg，.png 等 ) 的请求当作一个 Controller 请求，为其查询相应的处理器，当然是找不到的。

**组件配置**：

**springmvc.xml**：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
    <!--处理器映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--JSTL-->
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"></property>
        <!--后缀-->
        <property name="suffix" value=".jsp"></property>
    </bean>

</beans>
```

**注册处理器**：

**springmvc.xml**：

```xml
<!--注册一个Controller处理器-->
<bean id="/hello.do" class="com.lizi.controller.HelloController"></bean>
```

#### 静态资源访问请求

**1.DispatcherServlet 后缀匹配方式**：

```xml
<servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>*.do</url-pattern>
</servlet-mapping>
```

**2.Tomcat DefaultServlet 处理静态资源**

DefaultServlet 专门用于静态资源访问。该 Servlet 注册在 Tomcat 的 web.xml 中，在项目中直接使用即可。

```xml
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.css</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.js</url-pattern>
</servlet-mapping>
<servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.jpg</url-pattern>
</servlet-mapping>
```

**3.springmvc.xml 的 `<mvc:default-servlet-handler>`**：

`<mvc:default-servlet-handler/>` 将静态资源访问请求添加到 SimpleUrlHandlerMapping 的 urlMap 中，key 是 请求 URI，value 是默认请求处理器 DefaultServletHttpRequestHandler 对象，调用 DefaultServlet 处理静态资源访问请求。

```xml
<!--静态资源访问-->
<mvc:default-servlet-handler/>
```

**4.springmvc.xml 的 `<mvc:resource>`**：

Spring3.0.4定义了 `<mvc:resource>` 和专用于处理静态资源访问请求的处理器 ResourceHttpRequestHandler。

```xml
<mvc:resources mapping="/img/**" location="/img/"/>
```

> - mapping：表示以/static开头的所有请求路径，如/static/a 或者/static/a/b；
> - location：表示webapp目录下的static包下的所有文件；

### HandlerMapping 处理器映射器 

HandlerMapping 处理器映射器 ，主要完成 **请求 URL** 与 **处理器 Controller **的映射。

#### BeanNameUrlHandlerMapping

BeanNameUrlHandlerMapping 使用请求的 URL 与处理器 Spring bean 的 name 属性值进行匹配，映射到相应处理器。

- 请求URL：Spring bean 的 name 属性值。
- 处理器 Controller：class 属性值。

```xml
<bean name="/hello.do" class="org.lanqiao.contrller.HelloContrller"></bean>
```

#### SimpleUrlHandlerMapping

SimpleUrlHandlerMapping 使用请求的 URL 与 prop 的 key 属性值匹配，再通过该 prop 内容匹配到某个处理器 Spring bean 的 id，实现请求 URL 到 Controller 的映射。

- 请求URL：prop 的 key 属性值。
- 处理器 Controller：prop 内容映射 Controller 的 Spring bean 的 id。

mappings：

```xml
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
   <property name="mappings">
        <props>
            <prop key="hello.do">helloController</prop>
        </props>
    </property>
</bean>
<!--SimpleUrlHandlerMapping 注册HelloController处理器-->
<bean id="helloController" class="com.lizi.controller.HelloController"></bean>
```

urlMap：

```xml
<!--注册处理器映射器SimpleUrlHandlerMapping-->
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
    <property name="urlMap">
        <props>
            <prop key="/hello.do">helloController</prop>
        </props>
    </property>
</bean>
<!--注册HelloController处理器-->
<bean id="helloController" class="com.lizi.controller.HelloController"></bean>
```

### HandlerAdapter 处理器适配器

HandlerAdapter 处理器适配器，作用就是调用具体的方法对用户发来的请求来进行处理。当handlerMapping获取到执行请求的controller时，DispatcherServlte会根据controller对应的controller类型来调用相应的HandlerAdapter来进行处理。

#### SimpleControllerHandlerAdapter

SimpleControllerHandlerAdapter：所有实现了 Controller 接口的处理器，都是通过该适配器进行适配、执行的。

#### HttpRequestHandlerAdapter

HttpRequestHandlerAdapter：所有实现了 HttpRequestHandler 接口的处理器，都是通过该适配器进行适配、执行的。



### Handler 处理器

**1.实现 Controller 接口**：

```java
public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response);
```

 **2.实现 HttpRequestHandler 接口**：

```java
public void handleRequest(HttpServletRequest request, HttpServletResponse response)
```

**3.继承 AbstractController **：

```java
public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response);
```

继承 AbstractController 后，可以设置Http数据提交方式，默认支持 GET /  POST。

```xml
<bean id="helloController" class="com.lizi.controller.HelloController">
    <property name="supportedMethods" value="GET"></property>
</bean>
```

### ModelAndView

**ModelAndView**：

ModelAndView 指数据与视图的封装对象，负责设置页面跳转地址，封装数据到 request ，传递给视图。

```java
ModelAndView  modelAndView  = new ModelAndView(String viewName, String modelName, Objecct modelObject);
```

**Model**：接口

```java
model.addAttribute("attrName", attr)
```

**ModelMap**：实现类

```
modelMap.addAttribute("attrName", attr)
```

### ViewResolver 视图解析器

ViewResolver 视图解析器，负责将处理结果的逻辑视图名解析为具体视图。

#### InternalResourceViewResolver

InternalResourceViewResolver：当前web应用内部资源的封装与跳转。

- 资源路径：拼串，前缀+视图名称+后缀。
- 模型属性：存放到 request 域中。

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
    <!--前缀-->
    <property name="prefix" value="/WEB-INF/jsp/"></property>
    <!--后缀-->
    <property name="suffix" value=".jsp"></property>
</bean>
```

#### BeanNameViewResolver

BeanNameViewResolver：通过视图 bean 的 id 实现对视图资源的访问。

- 资源路径：视图 bean 的 id。
- 模型属性：存放到 request 域中。

```xml
<bean class="org.springframework.web.servlet.view.BeanNameViewResolver"></bean>
<!--注册视图bean：myInterView-->
<bean id="myInterView" class="org.springframework.web.servlet.view.JstlView">
    <property name="url" value="/WEB-INF/jsp/index.jsp"></property>
</bean>
<!--注册视图bean：baidu-->
<bean id="baidu" class="org.springframework.web.servlet.view.RedirectView">
    <property name="url" value="http://www.baidu.com"></property>
</bean>
```

#### XmlViewResolver

XmlViewResolver：将 springmvc.xml 中的 view bean 抽离出来，封装到一个 xml 文件中，使用该 xml 文件注册视图。使用 XmlViewResolver 定位视图对象的xml文件。

```xml
<bean class="org.springframework.web.servlet.view.XmlViewResolver">
    <property name="location" value="classpath:myView.xml"></property>
</bean>
```

#### ResourceBundleViewResolver

ResourceBundleViewResolver：使用 properties 文件注册视图。

```xml
<bean class="org.springframework.web.servlet.view.ResourceBundleViewResolver">
    <property name="basename" value="myView"></property>
</bean>
```

## SpringMVC 注解开发

@Controller - 标注当前类为处理器

@RequestMapping：标注当前方法为处理器方法。

### @RequestMapping 请求映射

@RequestMapping - 标注处理器方法。

**请求URL**：`value` 属性

```java
@RequestMapping("/xxx.do")
```

> 资源路径通配符：
>
> - `*`：一级资源路径。例如：`/xxx/*/hello.do` 表示 xxx 与 hello.do 之间存在一级路径。
> - `**`：多级资源路径。例如：`/xxx/**/hello.do` 表示 xxx 与 hello.do 之间存在任意级路径。

**请求方式**：`method` 属性

```java
@RequestMapping(value = "/xxx.do",method = RequestMethod.GET)
```

> method：`RequestMethod.GET`，`RequestMethod.POST`，`RequestMethod.PUT`，`RequestMethod.DELETE`。

**请求参数**：`params` 属性

```java
@RequestMapping(value="/xxx.do",params={"name","!age"})
```

> 请求参数必须携带 name，不能携带 age。

```java
@RequestMapping(value="/xxx.do",params={"name=Tom","age!=16"})
```

> 请求参数必须携带 name，且 name 必须等于 Tom；必须携带 age，且 age不能等于16。

### Controller 方法返回值类型

**返回 ModelAndView**：请求转发

- 视图：`modelAndView.setViewName(String viewName)`；
- 数据：`modelAndView.addObject(String modelName, Object modelObject)`；

**返回 String**：请求转发

- 视图：`return viewName`；
- 数据：`request.setAttribute()`；

**返回 void**：AJAX

请求转发：

- 视图：`request.getRequestDispatcher(String viewURL).forward(request,response)`；
- 数据：`request.setAttribute()`；

AJAX：

- 数据：`response.getWriter().write( json )`；

**返回 Object**：AJAX

- 数据：`return Object`；

**@ResponseBody**：将 Controller 返回的对象，通过适当的 HttpMessageConverter 转换后，写入到响应体中。

### Controller 请求转发&重定向

**返回 ModelAndView**：

- 请求转发：`modelAndView.setViewName("forward:/WEB-INF/pages/xxx.jsp")`；
- 重定向：`modelAndView.setViewName("redirect:www.baidu.com")`；

**返回 String**：

- 请求转发：`return "forward:/WEB-INF/pages/xxx.jsp"`；
- 重定向：`return "redirect:www.baidu.com"`；



### Controller 方法参数获取

- 获取单个参数

- 获取参数整体对象

#### @RequestParam

**@RequestParam** 注解参数：获取请求参数，传入方法形参中。相当于 `requset.getParameter()` 获取URL参数和表单数据。

示例代码：获取请求参数 id ，传入方法形参中。

```java
@RequestMapping("/getStudentById.do")
public ModelAndView getStudentById(@RequestParam(value = "id",required = true) int id){
    Student student = service.getStudentById(id);
    ModelAndView modelAndView = new ModelAndView("student","stu",student);
    return modelAndView;
}
```

- `value ` 属性 - 参数名。
- `required` 属性 - 是否必须传入，布尔值。

#### @PathVariable

**@PathVariable** 注解参数：获取路径变量，传入方法形参中。获取URL模板  `/page/{id}` 中的 `{id}` 值。

示例代码：获取URL路径中 {stuId} 处的值。

```java
@RequestMapping("/student/{stuId}")  
public void getStudent(@PathVariable String stuId, Model model) {
	// do Some...
} 
```

#### @SessionAttributes

**@SessionAttributes** 注解 Controller：指定 Model 中的属性同步到 session 域中。

示例代码：将 Model 中的 attr1 和 attr2 放入 session 域中。

```java
@Controller
@RequestMapping("/demo")
@SessionAttributes(value={"attr1","attr2"})
public class Demo {
    
    @RequestMapping("index.do")
    public ModelAndView index() {
        ModelAndView mav = new ModelAndView("/index.jsp");
        mav.addObject("attr1", "attr1Value");
        mav.addObject("attr2", "attr2Value");
        return mav;
    }
    
    @RequestMapping("/index2.do")
    public ModelAndView index2(@ModelAttribute("attr1")String attr1, @ModelAttribute("attr2")String attr2) {
        ModelAndView mav = new ModelAndView("success.jsp");
        return mav;
    }
}
```

#### **@ModelAttribute**

**@ModelAttribute(“attributeName”)** 注解方法：该方法在处理器方法之前执行，方法返回值放入模型属性 / session 中。

**@ModelAttribute(“attributeName”)** 注解参数：将模型属性放入注解的参数中。

Model

ModelMap

`<mvc:annotation-driven>` 会自动注册 RequestMappingHandlerMapping 与 RequestMappingHandlerAdapter 两个 Bean。

### 中文乱码问题

**web.xml**：

```xml
<!--解决中文乱码-->
<filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## Spring 高级

### 类型转换器

（1）自定义转换器：实现 Converter 接口，重写 convert() 方法。

```java
public class DateConverter implements Converter<String,Date> {
    @Override
    public Date convert(String dateStr) {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date date = null;
        try {
            date = simpleDateFormat.parse(dateStr);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return date;
    }
}
```

（2）注册自定义转换器：spring-config.xml

```xml
<!--注册类型转换器-->
<bean id="dateConverter" class="com.lizi.crud.converter.DateConverter"/>
<!--注册类型转换服务-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters" ref="dateConverter"></property>
</bean>
<!--注册MVC注解驱动-->
<mvc:annotation-driven conversion-service="conversionService"/>
```

### 异常处理器

前提：自定义异常类：继承 Exception。

```java
public class UserException  extends  Exception{
    public UserException(){
        super();
    }
    public UserException(String msg){
        super(msg);
    }
}

```

#### SimpleMappingExceptionResolver

直接注册异常处理器：spring-config.xml

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <prop key="com.lizi.crud.exception.UserException">UserException</prop>
        </props>
    </property>
    <!--默认错误页面-->
    <property name="defaultErrorView" value="/defaultError.jsp"></property>
    <!--错误信息-->
    <property name="exceptionAttribute" value="ex"></property>
</bean>
```

#### 自定义异常处理器

（1）自定义异常处理器：实现 HandlerExceptionResolver 接口，重写 resolveException() 方法。

```java
public class MyExceptionHandler implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("e",e);
        if(e instanceof UserException){
            modelAndView.setViewName("UserException");
        }
        return modelAndView;
    }
}
```

（2）注册异常处理器：spring-config.xml

```xml
<bean class="com.lizi.crud.exception.MyExceptionHandler"></bean>
```

#### 注解处理异常

定义 ExceptionHandlerController

```java
@Controller
public class ExceptionHandlerController {
    @ExceptionHandler(value = UserException.class)
    public ModelAndView handlerException(HttpServletRequest request, HttpServletResponse response, Exception e){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("defalutError");
        modelAndView.addObject("e",e);
        if(e instanceof UserException){
            modelAndView.setViewName("UserException");
        }
        return modelAndView;
    }
}
```

继承 ExceptionHandlerController

```java
public class UserController extends ExceptionHandlerController{}
```

### Validator 校验器

引入依赖：

```xml
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.0.7.Final</version>
</dependency>
```

注册 Validator 校验器

```xml
<bean id="myValidator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
    <property name="providerClass" value="org.hibernate.validator.HibernateValidator"></property>
</bean>
<mvc:annotation-driven validator="myValidator" />
```

**Bean Validation 中内置的 constraint**：

| Constraint                    | 详细信息                                                 |
| ----------------------------- | -------------------------------------------------------- |
| `@Null`                       | 被注释的元素必须为 `null`                                |
| `@NotNull`                    | 被注释的元素必须不为 `null`                              |
| `@AssertTrue`                 | 被注释的元素必须为 `true`                                |
| `@AssertFalse`                | 被注释的元素必须为 `false`                               |
| `@Min(value)`                 | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| `@Max(value)`                 | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| `@DecimalMin(value)`          | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| `@DecimalMax(value)`          | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| `@Size(max, min)`             | 被注释的元素的大小必须在指定的范围内                     |
| `@Digits (integer, fraction)` | 被注释的元素必须是一个数字，其值必须在可接受的范围内     |
| `@Past`                       | 被注释的元素必须是一个过去的日期                         |
| `@Future`                     | 被注释的元素必须是一个将来的日期                         |
| `@Pattern(value)`             | 被注释的元素必须符合指定的正则表达式                     |

**Hibernate Validator 附加的 constraint**：

| Constraint  | 详细信息                               |
| ----------- | -------------------------------------- |
| `@Email`    | 被注释的元素必须是电子邮箱地址         |
| `@Length`   | 被注释的字符串的大小必须在指定的范围内 |
| `@NotEmpty` | 被注释的字符串的必须非空               |
| `@Range`    | 被注释的元素必须在合适的范围内         |

@Validated 注解参数进行校验。

BindingResult 用于获取所有异常信息。

```java
FiledError xxError = br.getFiledError("XXError")
```

```el
${xxError.defaultMessage}
```



文件上传



拦截器





### RESTful

状态转换：

- GET：获取资源
- POST：新增资源
- PUT：更新资源
- DELETE：删除资源

示例：

–/order/1 HTTP GET ：得到 id = 1 的 order

–/order/1 HTTP DELETE：删除 id = 1的 order

–/order/1 HTTP PUT：更新id = 1的 order

–/order HTTP POST：新增 order