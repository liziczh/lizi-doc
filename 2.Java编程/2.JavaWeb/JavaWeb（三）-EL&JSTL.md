---
title: JavaWeb | EL&JSTL
comments: true
date: 2018-07-18 10:45:55
id: javaweb-el&jstl
tags: 
- JavaWeb
categories: JavaWeb
toc: true
reward: true
copyright: true
---

# EL&JSTL

避免页面出现大量 Java 代码，使用 EL&JSTL 替换 JSP 页面中的 Java 脚本表达式

<!--more-->

## EL 表达式

EL 表达式用于获取数据，替换 JSP 页面中的脚本表达式。

EL 表达式访问 JavaBean 属性

`${empty param.name}`：`null` 与 `""` 都返回 `true`；





### EL 表达式11个隐含对象

- pageContext
- pageScope
- requestScope
- sessionScope
- applicationScope
- param
- paramValues
- header
- headerValues
- cookie
- initParam







## JSTL

JSTL（JSP Standard Tag Library，JSP标准标签库），为弥补 html 的不足，规范自定义标签。

- 核心标签库
- I18 标签库
- SQL 标签库
- XML 标签库
- 函数标签库



jstl 导入jar包，引入标签库

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/function" %>
```



常用核心库：

set，out，remove

c:if，c:choose，c:when，c:otherwise，test=""

c:forEach，item，var





