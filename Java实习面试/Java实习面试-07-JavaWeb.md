# JavaWeb

### Servlet生命周期

- Servlet 通过调用 **init ()** 方法进行初始化。
- Servlet 调用 **service()** 方法来处理客户端的请求。
- Servlet 通过调用 **destroy()** 方法终止（结束）。
- 最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的。

### 请求转发与重定向的区别

- 浏览器显示：重定向会改变URL地址，请求转发不会改变URL地址。
- 资源共享：重定向不可以进行资源共享，请求转发可以资源共享。
- 功能：重定向可以用URL绝对路径访问其他Web服务器的资源，而请求转发只能在一个Web应用程序内进行资源转发，即服务器内部的一种操作。
- 效率：重定向效率低，相当于再一次请求；请求转发效率相对较高，跳转仅发生在服务器端。

### 静态包含与动态包含的区别

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

### Cookie&Session

**Cookie 机制**：

- Cookie 机制采用在客户端保持 HTTP 状态信息的方案实现会话跟踪。
- Cookie 是指在浏览器访问 Web 服务器时，Web 服务器在 **HTTP 响应头**中附带的一个小文本文件。Cookie存储在客户端上，保留了各种跟踪信息。其中，会话Cookie保存在内存中，持久Cookie保存在磁盘中。
- Cookie 机制：①服务器脚本向客户端浏览器发送一组 Cookie；②客户端浏览器将这些信息存储在本地计算机上；③当下一次浏览器向 Web 服务器发送请求时，浏览器会将这些 Cookie 信息发送到服务器，服务器通过这些 Cookie 信息识别用户。
- Cookie 底层原理：Web 服务器在 HTTP 响应中增加 `Set-Cookie` 响应头字段将 Cookie 发送给浏览器；浏览器通过在 HTTP 请求中增加 Cookie 请求头字段将 Cookie 回传给服务器。

**Session 机制**：

- Session 机制采用在服务器端记录客户端会话状态的方案保持会话状态。
- Session 机制：①在客户端浏览器第一次访问服务器时，Web 服务器为客户端浏览器创建一个会话对象（session 对象），并生成一个对应的 SessionID，服务器把客户端会话状态记录在用户独享的 session 对象中。②在客户端再次访问时，服务器根据客户端携带的 SessionID 从 session 域中查找用户的信息。

### Ajax

```js
$.ajax({
    type: "GET",
    url: "test.json",
    data: {username:"scott", content:"tiger"},
    dataType: "json",
    success: function(data){
    	// doSomething...
    }
});
```

### 跨域：CORS

```java
response.setHeader("Access-Control-Allow-Origin","*");
```

> 支持 GET 和 POST 请求



