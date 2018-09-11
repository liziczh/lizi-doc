# SSM

## MyBatis

### MyBatis的ORM原理



## Spring

### Spring IOC

IOC/DI（控制反转/依赖注入），在传统开发中，使用new关键字创建对象，程序主动去创建对象，程序耦合度变高；而在Spring中，由Spring容器管理对象，主动负责控制对象的生命周期和对象间的关系，程序被动接受。即由IoC容器帮对象找相应的依赖对象并注入，而不是由对象主动去找。

### Spring AOP

AOP（面向切面编程），将交叉业务逻辑织入到主业务逻辑中。底层是使用动态代理模式实现。

### Spring AOP 事务管理



## SpringMVC

SpringMVC 的加载流程：

1. 客户端发送请求到 DispatcherServlet（中央调度器）。
2. DispatcherServlet 查询 HandlerMapping（处理器映射器），找到处理请求的 Controller（处理器）。
3. DispatcherServlet 将请求转发给 Controller，Controller 处理请求，返回 ModelAndView（实体与视图）。
4. DispatcherServlet 查询 ViewResolver（视图解析器），找到 ModelAndView 指定的视图，渲染显示到客户端。