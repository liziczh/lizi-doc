---
title: JavaWeb | Filter
comments: true
date: 2018-07-22 21:02:16
id: JavaWeb-Filter
tags: 
- JavaWeb 
categories: JavaWeb
toc: true
reward: true
copyright: true
---

# Filter

Filter 过滤器，使用 Filter 可以动态拦截请求和响应，可以对 web 服务器管理的所有 web 资源（Servlet、JSP、静态图片或静态 html 等）进行拦截，从而实现一些特殊的功能。如 URL 级别的权限访问控制、过滤敏感词汇、压缩响应信息等一些高级功能。

<!--more-->

## Servlet Filter

### Filter 接口的方法

- `init(FilterConfig config)` - Filter 初始化。
- `doFilter(ServletRequest, ServletResponse, FilterChain)` - 实现过滤操作，Web 服务器每次调用 service() 方法前都会先调用 Filter 的 doFilter() 方法，FilterChain 用于访问后续 Filter。
- `destroy()` - 销毁 Filter 前调用。

### doFilter() 方法详解

Web 服务器每次调用 Servlet 的 service() 方法前都会先执行其 Filter 的 doFilter() 方法。

```java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
    // 预处理：在客户端请求调用Web资源前执行。
    System.out.println("doFilter() 方法前执行");
    // 如果存在后续Filter，则根据过滤链传递给下一过滤器处理，
    // 若Filter全部处理完毕，则访问Web资源。
    chain.doFilter(req, resp);
    // 后处理：调用Web资源后执行
    System.out.println("doFilter() 方法后执行");
}
```

Filter 拦截：

- 预处理：在客户端的请求访问后端资源之前，拦截这些请求。
- 后处理：在服务器的响应发送回客户端之前，处理这些响应。

### Filter 链

Filter 链由多个 Filter 按先后顺序组合起来形成。

Filter 链调用顺序由 `web.xml` 中的**映射顺序**决定。



## Filter 配置

在 web.xml 中配置 Filter 信息

```xml
<!-- 1. 注册Filter -->
<filter>
    <filter-name>firstFilter</filter-name>
    <filter-class>com.lizi.filter.FirstFilter</filter-class>
</filter>
<!-- 2. Filter映射 -->
<filter-mapping>
    <filter-name>firstFilter</filter-name>
    <!-- 通配符*表示拦截所有请求 -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

使用注解配置



## FilterConfig



## Filter 应用

### 字符编码过滤



### 检查用户是否登陆



### 权限管理

用户、角色、权限、用户角色关系、角色权限关系。

