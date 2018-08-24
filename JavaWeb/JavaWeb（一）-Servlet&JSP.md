---
title: JavaWeb | Servlet&JSP
comments: true
date: 2018-06-22 12:35:34
id: javaweb-ServletAndJsp
tags: 
- JavaWeb
categories: JavaWeb
toc: true
reward: true
copyright: true
---

JavaWeb-Servlet

<!--more-->

# JavaWeb后端

- Servlet
- JSP
- MVC
- JavaWeb会话与状态管理
- EL&JSTL
- 过滤器Filter
- 监听器Listener
- Web文件上传下载
- 国际化
- 目标：实现Web网上商城
- AJAX：局部刷新

## JavaWeb概述

静态web资源：html

动态web资源：JSP/Servlet、ASP、PHP。

JavaWeb：动态Web资源开发技术。

JavaWeb三大组件：Servlet，Filter，Listener；（全属于动态资源）

Servlet容器：Tomcat，确保JAVA_HOME配置正确

## Tomcat

Dynamic Web Project，New Server：location：tomcat安装目录下wtpwebapp；

配置`server.xml`的`Context`：

```xml
<Context docBase="D:\Code\Java\Tomcat\apache-tomcat-9.0.8\wtpwebapps\MyWeb" path="/" reloadable="true" source="org.eclipse.jst.jee.server:MyWeb"/>
```

### 虚拟目录配置

在`tmocat\conf\Catalina\localhost`下，自定义一个xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context docBase="D:\Code\Java\WebApp\MyWeb" path="/" reloadable="true" source="org.eclipse.jst.jee.server:MyWeb"/>
```

### 虚拟主机配置

基于主机名的虚拟主机配置：在server.xml中配置多个Host。

基于端口号的虚拟主机配置：在server.xml中配置多个Service。

### Web项目打包

war包：可以直接拷贝到webapps目录中，会自动解压直接使用tomcat运行。



## HTTP协议

HTTP超文本传输协议。请求协议+响应协议。请求响应模式

HTTP特点：无连接，无状态。

GET（查）、POST（改）、PUT（增）、DELETE（删）。

- **GET** - 从指定的资源请求数据。一般用于**查询资源信息**。
- **POST** - 向指定的资源提交要被处理的数据。一般用于**更新资源信息**。 

| 操作方式 | GET                     | POST     |
| -------- | ----------------------- | -------- |
| 数据位置 | HTTP包头                | HTTP正文 |
| 数据加密 | 明文（数据暴露在URL中） | 可明可密 |
| 数据安全 | 不安全                  | 安全     |
| 数据长度 | 1KB以下                 | 无限制   |
| 应用场景 | 查询数据                | 修改数据 |

### ？HTTPS

**1.HTTPS协议是什么？**

在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。 

https协议需要到ca申请证书，一般免费证书很少，需要交费。 

http是超文本传输协议，信息是明文传输，https 则是具有安全性的SSL加密传输协议。 

http协议使用80端口，https协议使用443端口。

https协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议。

**2.HTTPS常见加密算法有哪些？**

- **非对称加密算法**（密钥对，客户端用公钥加密数据发送给服务端，服务端用私钥解密数据）：DH、RSA、EL GAML、ECC 

  RSA：是目前最有影响力的公钥加密算法，已被ISO推荐为公钥数据的加密标准。其背后复杂的数学原理，简单来说就是：将2个大素数（质数）相乘非常容易，但要对其乘积结果因式分解非常困难。 

- **对称加密算法**（只有一个密钥，加密和解密都用这个密钥）：DES、IDEA、RC2、RC4、SKIPJACK 
- **HASH算法**，常用的摘要算法：MD5、SHA1、SHA256 

**3.如何在tomcat中开启HTTPS协议？**

在jdk的安装目录\bin\keytool.exe下打开keytool.exe

在命令行中输入以下命令：

```shell
keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore "g:\tomcat.keystore"
```

配置server.xml：

```xml
<Connector port="443" protocol="org.apache.coyote.http11.Http11Protocol" SSLEnabled="true" 
		maxThreads="150" scheme="https" secure="true"  
		clientAuth="false" sslProtocol="TLS"
		keystoreFile="d:\tomcat.keystore"
		keystorePass="123456" />
```

## web.xml 加载过程

1.启动 web 项目，创建一个 ServletContext（Servlet 上下文），即应用域。

2.Web 容器（Tomcat）读取 web.xml 中的 `<context-param>` 键值对到 ServletContext。

3.Web 容器读取 web.xml 中的 `<listener>` 创建监听器。

4.Web 容器读取 web.xml 中的 `<filter>` 创建过滤器。

5.Web 容器读取 web.xml 中的 `<servlet>` 创建 Servlet。

## Servlet

Servlet用于处理请求，属于动态资源。

Servlet处理请求过程：

- 接收请求数据
- 处理请求
- 完成响应

Servlet容器：Tomcat；

### 创建Servlet

**创建Servlet三种方式**：

1. 实现 Servlet 接口
2. 继承 GenericServlet 类
3. 继承 HttpServlet 类

**Servlet接口的方法**：

- `init()` - 用于初始化，只执行一次。
- `service()` - 用于处理请求，请求一次执行一次。
- `destory()` - 用于完成扫尾工作，当Servlet从容器中卸载时执行。
- `getServletConfig()`
- `getServletInfo()`

### Servlet生命周期

- Servlet 通过调用 **init ()** 方法进行初始化。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 通过调用 **destroy()** 方法终止（结束）。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

### Servlet配置

当通过浏览器去访问localhost:8080时，tomcat会首先解析web.xml。

```xml
	<!--在Servlet容器中注册Servlet-->
    <servlet>
        <!--Servlet名称-->
        <servlet-name>firstName</servlet-name>
        <!--Servlet类名-->
        <servlet-class>com.lizi.web.FirstServlet</servlet-class>
        <!--配置servlet的初始化参数-->
        <init-param>
            <!--参数名称-->
            <param-name>hello</param-name>
            <!--参数值-->
             <param-value>world</param-value>
        </init-param>
        <!--servlet的初始化的时机，
        负数，在第一次请求时对servlet初始化
        0/正数，在servlet容器启动时对servlet初始化（正数越小，初始化越早）
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>
```

```xml
<!--Servlet映射-->
<servlet-mapping>
    <!--Servlet名称-->
    <servlet-name>firstName</servlet-name>
    <!--对应的url地址-->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

**url-pattern配置**：

```xml
<!--通常使用通配符*配置url-->
<url-pattern>*.do</url-pattern>
<url-pattern>*.action</url-pattern>
```

ServletRequest servletRequest：封装了请求信息（请求头+请求体）；

ServletResponse servletResponse ：封装了响应信息（响应头+响应体）；

### GenericServlet

GenericServlet类：抽象service方法，实现其他方法。

继承GenericServlet类：只需实现service方法即可。

### Servlet 3.0 配置

在开发中两种配置方式：约定 > 配置。

- 使用**注解**配置开发Servlet；
- 使用**xml**配置开发Servlet；

**Servlet 3.0支持注解配置**：约定 > 配置；

```java
@WebServlet(name = "ServletDemo2" , urlPatterns = {"/demo2","/hello" })
@WebServlet("/demo2")   // 未定义参数名，则默认为 urlPatterns。
```

@WebServlet 约定这是一个Servlet。

### HttpServlet

HttpServlet对http协议提供了特殊支持。

- doGet() - Get请求时执行
- doPost() - Post请求时执行

#### 请求类型

URL请求类型为GET类型。

将请求参数拼接在URL后边 `url？name=val&name=val`；

一个Servlet同时执行多种请求：`URL?method=delete`；



Form表单可指定请求类型 (GET/POST) 。

```html
<form action="" method="GET/POST">
	表单域：表单元素；
</form>
```

文件上传使用POST请求 (文件大小无限制) ；

**获取请求表单数据**：

```java
String getParameter(String name);  // 通过name获取参数值
String[] getParameterValues(String name);  // 通过name获取所有同名参数的value属性值
```

## Servlet API

### ServletConfig

ServletConfig 对象由 Servlet 独享。

1.获取 ServletName

```java
String getServletName();
```

2.获取初始化参数值 By 初始化参数 Name

```java
String getInitParameter(String name);
```

3.获取所有初始化参数 Name

```java
Enumeration<String> getInitParameterNames();
```

枚举 (enum)：有穷可列举。

4.获取 ServletContext

```java
ServletContext getServletContext();
```

### ServletContext

ServletContext 的生命周期贯穿整个web应用，(域对象)。

ServletContext 数据由一个web应用中的所有Servlet共享，(Application域)。

```xml
<context-param>
	<param-name>appname</param-name>
	<param-value>app</param-value>
</context-param>
```

**获取ServletContext对象**：

- ServletConfig对象调用getServletContext()方法；
- HttpServlet子类对象调用getServletContext()方法；

**ServletContext域属性操作**：

```java
void setAttribute(String name, Object value);
Object getAttribute(String name);
void removeAttribute(String name);
```

获取资源：

```java
String getRealPath(String filepath);  // 获取资源物理路径
InputStream getResourceAsStream(String filepath);  // 获取文件流
```

### Request

每一次请求创建一个request对象。（域对象）

request对象生命周期：发出请求到请求结束。

**获取请求头**：

```java
String getHeader(String name);  // 获取指定name的请求头
Enumeration getHeaderNames();  // 获取所有请求头name
int getIntHeader(String name);  //获取int类型值的请求头
```

**获取请求URL信息**：

```java
int getContentLength()  // 获取请求体的字节数，GET请求没有请求体，没有请求体返回-1；
String getContentType()  // 获取请求类型，如果请求是GET，那么这个方法返回null；如果是POST请求，那么默认为application/x-www-form-urlencoded，表示请求体内容使用了URL编码；
String getMethod()  // 返回请求方法，例如：GET
Locale getLocale()  // 返回当前客户端浏览器的Locale。java.util.Locale表示国家和言语，这个东西在国际化中很有用；
String getCharacterEncoding()  //获取请求编码，如果没有setCharacterEncoding()，那么返回null，表示使用ISO-8859-1编码；
void setCharacterEncoding(String code)  // 设置请求编码，只对请求体有效！注意，对于GET而言，没有请求体！！！所以此方法只能对POST请求中的参数有效！
String getContextPath()  // 返回上下文路径，例如：/hello
String getQueryString()  // 返回请求URL中的参数，例如：name=zhangSan
String getRequestURI()  // 返回请求URI路径，例如：/hello/oneServlet
StringBuffer getRequestURL()  // 返回请求URL路径，例如：http://localhost/hello/oneServlet，即返回除了参数以外的路径信息；
String getServletPath()  // 返回Servlet路径，例如：/oneServlet
String getRemoteAddr()  // 返回当前客户端的IP地址；
String getRemoteHost()  // 返回当前客户端的主机名，但这个方法的实现还是获取IP地址；
String getScheme()  // 返回请求协议，例如：http；
String getServerName()  // 返回主机名，例如：localhost
int getServerPort()  // 返回服务器端口号，例如：8080
```

**获取请求参数**：

```java
String getParameter(String name); // 获取指定名称的参数值
String[] getParameterValues(String name); // 获取多个同名参数值
Enumeration<String> getParameterNames(); // 获取参数名称
```

### Response

#### 设置响应正文

字符流

字节流

在一个请求中，不能同时使用两个流。

#### 设置响应头



#### 设置状态码



### Servlet请求转发

#### 请求转发

请求转发：保存当前请求的request和response对象，转发到指定URL处理。请求转发共享一个request和一个response对象，只发送一次请求，页面不跳转（地址栏不发生改变）。

```java
request.getRequestDispatcher("/servletDemo2").forward(request,response);
```

#### 重定向

重定向：

```java
response.sendRedirect(String location);
```

客户端发送了两次请求，地址栏发生变化。

重定向第二个请求一定是GET。

#### 请求包含

```java
request.getRequestDispatcher("/servletDemo2").include(request,response);
```

请求包含多用于JSP页面，完成多页面的合并。

请求转发多用于Servlet中，转发目标大多是JSP页面。



**Servlet 中 forward() 与 redirect() 的区别？**

- 浏览器显示：重定向会改变URL地址，请求转发不会改变URL地址。

- 资源共享：重定向不可以进行资源共享，请求转发可以资源共享。

- 功能：重定向可以用URL绝对路径访问其他Web服务器的资源，而请求转发只能在一个Web应用程序内进行资源转发，即服务器内部的一种操作。

- 效率：重定向效率低，相当于再一次请求；请求转发效率相对较高，跳转仅发生在服务器端。



### 中文乱码问题

中文乱码产生原因：编码解码字符集不一致。

**POST请求中文乱码**解决方案：在获取各种参数之前`setCharacterEncoding("UTF-8")`；

```java
request.setCharacterEncoding("UTF-8");
```

**GET请求中文乱码**解决方案：在Tomcat中server.xml中设置`URIEncoding = "UTF-8"`；

```xml
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8" / >
```

**字符串终极解决方案**：如果以上两种配置都没用，重新编码；

```java
String name = request.getParameter("name");
name = new String(name.getBytes("iso-8859-1"),"UTF-8");
```

**响应内容中文乱码**解决方案：

```java
response.setContentType("text/html;charset=utf-8");  // 设置响应内容的字符集
```

## JSP

JSP（Java Server Pages，Java 服务器页面），是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码。 

JSP本质是Java Servlet。将jsp部署到Web容器后，jsp文件会被翻译为index_jsp.java文件。

JSP通过网页表单获取用户输入数据、访问数据库及其他数据源，然后动态地创建网页。 

JSP = html +  java代码片段 + jsp动态标签

### JSP 语法

**JSP 注释**：

```jsp
<%--注释内容--%>
```

**JSP 表达式**：

```jsp
<%= Java表达式 %>
```

> JSP表达式不加分号，Java表达式被翻译成一条 `out.print()` 语句。

**JSP 声明**：

```jsp
<%! Java定义类成员 %>
```

**JSP 脚本片段**：

```jsp
<% Java代码片段 %>
```

> JSP脚本片段可以直接使用JSP提供的隐式对象完成WebAPP特定功能。
> JSP脚本片段中的Java代码将原样搬进由JSP页面翻译成的Servlet中的jspService方法中。

### JSP 九大隐含对象★

**隐含对象/内置对象**：在jsp页面上无需显式声明，即可直接使用。（在jsp转换为servlet时，隐含对象已被声明在servlet的 `_jspService` 方法中）

| JSP 隐含对象  | 描述                                             |
| ------------- | ------------------------------------------------ |
| `out`         | PrintWriter类的对象，用于将内容响应在页面上      |
| `pageContext` | ServletContext类的对象，获取当前页面的所有对象   |
| `session`     | HttpSession类的对象，表示一次会话                |
| `page`        | 表示当前页面对象（this）                         |
| `config`      | ServletConfig类的对象，表示当前servlet的配置信息 |
| `application` | ServletContext类的对象，表示当前应用上下文       |
| `request`     | HttpRequest类的对象，表示一次请求                |
| `response`    | HttpResponse类的对象，表示一次响应               |
| `exception`   | Exception类的对象，表示当前页面的异常对象        |

> exception对象必须在 `isErrorPage="true"` 的情况下使用。

### JSP 域对象

**JSP 四个域对象**：pageContext，request，session，application

| JSP域对象   | 作用范围  | 描述                             |
| ----------- | --------- | -------------------------------- |
| pageContext | page域    | 只能在当前jsp页面使用（当前页面) |
| request     | request域 | 只能在同一个请求中使用（转发）   |
| session     | session域 | 只能在同一个会话中使用 （私有）  |
| application | context域 | 只能在一个web应用中使用（全局）  |

> 一次会话：浏览器打开直到浏览器关闭。

**域对象属性操作**：

```java
// 设置域对象属性
void setAttribute(String name, Object value);
// 获取域对象属性
Object getAttribute(String name);
// 移除域对象属性
void removeAttribute(String name);
```

**域对象的作用**：保存数据，获取数据，数据共享。

### JSP 指令

JSP指令用来设置整个JSP页面相关的属性，告诉JSP引擎如何处理JSP页面其他部分。

**基本语法**：

```jsp
<%@ 指令 属性名="值" %>
```

**示例**：

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
```

**指令类型**：

| 指令标签             | 描述             |
| -------------------- | ---------------- |
| `<%@ page ... %>`    | 定义网页依赖属性 |
| `<%@ include ... %>` | 包含其他文件     |
| `<%@ taglib ... %>`  | 引入标签库       |

#### page指令

**page指令**：为JSP页面的定义各种属性。

| 常用属性                                    | 描述                                |
| ------------------------------------------- | ----------------------------------- |
| `import="{packag.class \| package.*}, ..."` | 导包                                |
| `session="true | false"`                    | 指定当前页面是否生成隐含对象session |
| `contentType="mimeType;charset=字符集"`     | 指定MIME类型和字符编码              |
| `pageEncoding="字符集"`                     | 指定页面编码字符集                  |
| `isELIgnored="true | false"`                | 指定是否执行EL表达式                |
| `errorPage="relative_url"`                  | 指定错误处理页面的相对路径          |
| `isErrorPage="true | false"`                | 指定当前页面是否为错误处理页面      |

**errorPage与isErrorPage对比**：

- `errorPage` 属性值必须使用相对路径（`/`代表当前WEB应用的根目录）。响应errorPage使用请求转发方式。
- `isErrorPage` 指定当前页面是否为错误处理页面，当前页面是否隐含exception对象。

#### include 指令

**include 指令**：包含其他文件（静态包含），当前JSP页面与引入页面在被JSP引擎翻译成Servlet的过程中合并为一个Servlet。

```jsp
<%@ include file="relativeURL" %>
```

> 被引入文件必须遵循JSP语法。
> 被引入文件可以使用任意扩展名，建议使用.jspf。

### JSP 动作元素

JSP Action元素完成各种通用JSP页面功能，实现一些处理复杂业务逻辑的专用功能。

**`<jsp:include>`**：包含其他文件（动态包含）。将另一个页面的输出内容插入当前JSP页面的输出内容之中。

```jsp
<jsp:include page="relative URL" flush="true" />
```

> page属性指定引入资源的相对路径。
> flush属性定义在包含资源前是否刷新缓存区 

**`<jsp:forward>`**：请求转发

```jsp
<jsp:forward page="Relative URL" />
```

**`<jsp:param>`**：以键值对形式为其他标签提供附加信息。

`<jsp:param>`与`<jsp:include>`搭配使用：

```jsp
<jsp:include page="resource/sum.jsp">
    <jsp:param name="number" value="300" />
</jsp:include>
```



**静态包含与动态包含的区别**：

（1）语法不同：

- 静态包含：JSP指令 - `<%@ include file=""%> `
- 动态包含：JSP行为 - `<jsp: include page=""%> `

（2）生成文件数量不同：

- 静态包含：两个文件二合一，整体编译，生成一个servlet和class文件。
- 动态包含：各个jsp分别转换，分别编译，生成多个servlet和class文件。

（3）包含时机不同：

- 静态包含：JSP翻译成Servlet阶段。
- 动态包含：执行class文件阶段，动态加入。

（4）静态包含在两个文件中不能有相同的变量，动态包含允许

（5）静态包含只能包含文件，动态包含还可以包含servlet输出的结果

（6）静态包含不能使用变量作为文件名，动态包含可以使用变量作为文件名

（7）动态包含文件发生变化，包含文件会感知变化


