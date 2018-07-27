---
title: JavaWeb | Listener
comments: true
date: 2018-07-22 21:02:16
id: JavaWeb-Listener
tags: 
- JavaWeb 
categories: JavaWeb
toc: true
reward: true
copyright: true
---

# Listener

Listener 监听器，用于监听 ServletContext、HttpSession、ServletRequest 等域对象的初始化、销毁、属性变化事件。

<!--more-->

监听对象：

- ServletContext：application
- HttpSession：session
- ServletRequest：request

## Servlet Listener

ServletContextListener

HttpSessionListener

HttpSessionAttributeListener

ServletRequestListener

ServletRequestAttributeListener



### Listener XML 配置

使用 web.xml 配置

```xml
<listener>
    <listener-class>com.lizi.listener.IndexListener</listener-class>
</listener>
```

使用注解配置

```js
@WebListener("This is a Listener")
```