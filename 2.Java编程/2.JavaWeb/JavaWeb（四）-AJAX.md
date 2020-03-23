---
title: JavaWeb | AJAX
comments: true
date: 2018-07-19 21:02:16
id: JavaWeb-ajax
tags: 
- JavaWeb 
- AJAX
categories: JavaWeb
toc: true
reward: true
copyright: true
---

<!--# AJAX-->

AJAX（Asynchronous JavaScript and XML，异步的 JavaScript 和 XML）一种异步请求、局部刷新技术。它在不重新载入全部页面的情况下，与服务器交换数据，实现对部分页面的更新。

<!--more-->

- XMLHttpRequest 对象（异步的与服务器交换数据）
- JavaScript/DOM（信息显示/交互）
- CSS（给数据定义样式）
- XML（作为转换数据的格式）

AJAX 应用程序与浏览器和平台无关的！

## XMLHttpRequest

### XHR 创建对象

```js
function createXMLHttpRequest() {
  var xmlhttp;
  if (window.XMLHttpRequest) {
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp=new XMLHttpRequest();
  } else {
    // IE6, IE5 浏览器执行代码
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
}
```

```js
// 创建一个 XMLHttpRequest 对象
var xhr = createXMLHttpRequest();
```

### XHR 请求

（1）`open(String method, String url, boolean async)` - 打开与服务器的连接。

> - method：请求类型，GET / POST；
> - url：文件路径；
> - async：true (异步)，false (同步)，默认为 true；

（2）`send(String data)`  - 将请求的数据发送到服务器。

> - GET请求：`send()`，数据可直接通过 open() 中的URL地址传送。
> - POST请求：`send("key=value&key=value")`，数据以键值对形式发送。
>
> 如果是 POST 请求，则需要在 open() 打开链接后，设置请求头内容类型：
>
> ```js
> xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
> ```

**GET 请求发送数据示例**：

```js
// 打开与服务器的连接
xhr.open("GET","/getDemo.jsp?key1=value1&key2=value2",true);
// 发送数据，GET请求数据已包含在URL中，此处发送null即可。
xhr.send();
```

**POST 请求发送数据示例**：

```js
// 打开与服务器的连接
xhr.open("POST","/postDemo.jsp",true);
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
// POST请求数据以键值对形式发送
xhr.send("key=value&key=value");
```

### XHR 响应

（1）`responseText` - 获得字符串形式的响应数据。

（2）`responseXML` - 获得 XML 形式的响应数据。

**使用 responseText 获取响应数据示例**：

```js
document.getElementById("myDiv").innerHTML=xhr.responseText;
```

### XHR readyState

（1）`onreadystatechange` - 每当 readyState 改变时，触发 onreadystatechange 事件。

（2）`readyState` - 表示 XHR 的状态。

> - 0: 请求未初始化。对象已创建，未调用 open()；
> - 1: 服务器连接已建立。open() 成功调用，未调用 send()；
> - 2: 请求已接收。send() 成功调用，尚未开始接受数据；
> - 3: 请求处理中。正在接受数据，尚未接受完成；
> - 4: 请求已完成，且响应已就绪。

（3）`status` - HTTP 状态码（如 `200`: "OK"，`404`: "未找到页面"，`500`: “服务器内部错误”等）。

**readyState 示例**：

```js
xhr.onreadystatechange = function(){
  if (xmlhttp.readyState == 4 && xmlhttp.status == 200){
    document.getElementById("myDiv").innerHTML = xhr.responseText;
  }
}
```

### AJAX 异步请求流程

```js
function createXMLHttpRequest() {
  var xhr;
  if (window.XMLHttpRequest) {
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xhr=new XMLHttpRequest();
  } else {
    // IE6, IE5 浏览器执行代码
    xhr=new ActiveXObject("Microsoft.XMLHTTP");
  }
}
window.onload = function () {
  // 1. 创建一个 XMLHttpRequest 对象
  var xhr = createXMLHttpRequest();
  // 2. 打开与服务器的连接
  xhr.open("POST","/index.jsp",true);
  // -  POST 请求需要设置请求头
  xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded")
  // 3. 发送数据
  xhr.send("username=scott&password=tiger");
  // 4. 当响应完成&页面正常的情况下，接受响应数据，写入myDiv中。
  xhr.onreadystatechange = function(){
    if (xmlhttp.readyState == 4 && xmlhttp.status == 200){
      document.getElementById("myDiv").innerHTML = xhr.responseText;
    }
  }
}
```

## 数据格式

### HTML



### JSON 



## jQuery AJAX

### load()

`jQuery load()` ：从服务器加载数据，并将返回的数据放入元素中。

```js
$(selector).load(URL,[data],function(responseTxt,statusTxt,xhr));
```

> URL：加载内容的地址。
>
> data：规定与请求一同发送的查询字符串键/值对集合。 
>
> function(response,status,xhr)：load() 方法完成后所执行的回调函数。
>
> - responseTxt：调用成功后的结果内容
> - statusTxt：调用状态
> - xhr：XMLHttpRequest 对象

### get() & post()

（1）`$.get()` 方法通过 HTTP GET 请求从服务器上请求数据。

```js
$.get(URL,data,callback,type);
```

（2）`$.post()` 方法通过 HTTP POST 请求从服务器上请求数据。

```js
$.post(URL,data,callback,type);
```



## 跨域

同源策略：

### JSONP

```js
dataType: "jsonp",
jsonpCallback: "callback",
```

> 只支持 GET 请求 

### CORS

```java
response.setHeader("Access-Control-Allow-Origin","*");
```

> 支持 GET 和 POST 请求









