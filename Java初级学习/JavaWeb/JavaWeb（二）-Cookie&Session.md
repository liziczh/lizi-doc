---
title: JavaWeb | Cookie&Session
comments: true
date: 2018-07-16 20:45:55
id: javaweb-CookieAndSession
tags: 
- JavaWeb
categories: JavaWeb
toc: true
reward: true
copyright: true
---

<!--# Java会话与状态管理-->

由于 HTTP 是一种**无状态**协议，浏览器每一次请求都是独立的，无法维持客户端与Web 服务器之间的会话状态，所以引入**会话跟踪**技术，即 **Cookie** 和 **Session** 机制。

Web 应用中的**会话**指一个客户端浏览器与 Web 服务器之间连续发生的一系列请求与响应过程。**会话状态**指 Web 服务器与浏览器在会话过程中产生的状态信息。**SessionID** 用于唯一地标识一个会话的请求信息。

<!--more-->

## Cookie 机制

Cookie 机制采用在客户端保持 HTTP 状态信息的方案实现会话跟踪。

Cookie 是指在浏览器访问 Web 服务器时，Web 服务器在 **HTTP 响应头**中附带的一个小文本文件。Cookie存储在客户端上，保留了各种跟踪信息。其中，会话Cookie保存在内存中，持久Cookie保存在磁盘中。

Cookie 机制：①服务器脚本向客户端浏览器发送一组 Cookie；②客户端浏览器将这些信息存储在本地计算机上；③当下一次浏览器向 Web 服务器发送请求时，浏览器会将这些 Cookie 信息发送到服务器，服务器通过这些 Cookie 信息识别用户。

Cookie 底层原理：Web 服务器在 HTTP 响应中增加 `Set-Cookie` 响应头字段将 Cookie 发送给浏览器；浏览器通过在 HTTP 请求中增加 Cookie 请求头字段将 Cookie 回传给服务器。

### Servlet Cookie 

**Cookie 类**：

- `Cookie(String name, String value)` - Cookie 构造方法
- `getName()` - 获取 Cookie name
- `getValue()` 和 `setValue(String value)`  - 获取/设置 Cookie Value
- `getMaxAge()` 和 `setMaxAge(int expiry)` - 获取/设置 Cookie 最大生存周期
- `getPath()` 和 `setPath(String url)` - 获取/设置 Cookie 适用路径

**HttpServletResponse 添加 Cookie 到 HTTP 响应头中**：

```java
void response.addCookie(Cookie cookie)
```

**HttpServletRequest 获取当前请求的所有 Cookie**：

```java
Cookie[] request.getCookie()
```

### Cookie应用

#### 自动登陆

```

```

## Session 机制

Session 机制采用在服务器端记录客户端会话状态的方案保持会话状态。

Session 机制：①在客户端浏览器第一次访问服务器时，Web 服务器为客户端浏览器创建一个会话对象（session 对象），并生成一个对应的 SessionID，服务器把客户端会话状态记录在用户独享的 session 对象中。②在客户端再次访问时，服务器根据客户端携带的 SessionID 从 session 域中查找用户的信息。

### 保存 SessionID

Session 机制中客户端保存用户的 SessionID，用户的会话记录保存在服务器端，Web 服务器通过 SessionID 区分不同用户的会话状态。

保存 SessionID 的方式：

- 以 cookie 形式写回客户端。
- 当浏览器禁用 cookie 后，服务器采用 URL 重写：将 session_id 附在 URL 后面。

Session 持久化

```java
<%
	Cookie cookie = new Cookie("JSESSION",session.getId());
	cookie.setMaxAge(20);
	response.addCookie(cookie);
%>
```

### HttpSession

获取session对象：request.getSession()

getSession(true)：先创建session对象，再返回session。

获取sessionID：session.getId()







SessionID保存在Cookie中。

Session会话，保存在服务端，客户端只存在一个sessionID信息。

一般将jsp页面放在WEB-IFO下。



常用方法：

获取SessionID：

设置最大有效时间：

使session失效：invalidate()



### 销毁 Session

- `session.invalidate()` - 
- 设置的 session-timeout 的时间到了

```xml
<!-- session 配置 -->
<session-config>
    <!-- session 销毁时机 -->
    <session-timeout>5</session-timeout>
</session-config>
```

- 服务器卸载应用



### 利用URL重写实现session跟踪

当cookie被禁用时，如何跟踪session？

URL重写技术：将会话标识号以参数形式附加在URL地址后面

HttpServletResponse接口的方法：

- `encodeURL()`
- `encodeRedirectURL()`

URL重写实例：

```jsp
<a herf= <%=response.encodeURL("/hello_2.jsp")%> >hello_2.jsp</a>
```

### 请求的绝对路径

获取项目根目录的绝对路径：

- `request.getContextPath()`
- `application.getContextPath()`

绝对路径写法：

客户端（浏览器，JSP）：`${pageContext.request.contextPath}` 相当于 `/应用名` 。

服务器端（Servlet）：`/` 相当于 当前应用下。



### Session应用

#### 防止表单多次提交

（1）使用JavaScript实现防止表单重复提交：

- 提交一次，将submit按钮置为disabled。
- 绑定onSubmit事件。

（2）使用Session实现防止表单重复提交：

- Token对比

#### 购物车

- 使用Cookie存储
- 使用Session存储
- 使用Cookie+数据库存储（常用）









#### 一次性验证码











