# SSM

## MyBatis

### MyBatis的ORM原理

① 封装JDBC

② 利用反射机制实现Java类与SQL语句之间的转换。

MyBatis API：

- SqlSession：作为MyBatis工作的主要顶层API，表示和数据库交互时的会话，完成必要数据库增删改查功能。
- Executor：MyBatis执行器，是MyBatis 调度的核心，负责SQL语句的生成和查询缓存的维护。
- StatementHandler：封装了JDBC Statement操作，负责对JDBC statement 的操作，如设置参数等。
- ParameterHandler：负责对用户传递的参数转换成JDBC Statement 所对应的数据类型
- ResultSetHandler：负责将JDBC返回的ResultSet结果集对象转换成List类型的集合
- TypeHandler：负责java数据类型和jdbc数据类型(也可以说是数据表列类型)之间的映射和转换



## Spring

### Spring容器

实例化Spring容器：通过XML文件利用反射机制，Spring容器会预初始化所有类。

```
ApplicationContext ctx  = new ClassPathXmlApplicationContext("bean.xml");
```

### Spring IOC

基于Java反射实现。

IOC/DI（控制反转/依赖注入），在传统开发中，使用new关键字创建对象，程序主动去创建对象，程序耦合度变高；而在Spring中，由Spring容器管理对象，主动负责控制对象的生命周期和对象间的关系，程序被动接受。即由IoC容器帮对象找相应的依赖对象并注入，而不是由对象主动去找。

**Spring IOC 反射原理**：

类：解析XML文件，获取Bean的class，可以得到Bean的全类名，利用反射机制得到Bean的Class对象，实例化Bean对象，放入Spring容器中。

引用：解析XML文件，获取Bean的引用属性的Bean的ID，进而获取到相应的Bean，利用反射获取到setter()方法，将引用Bean注入进去。

**Spring IOC/DI**：

属性注入：调用setter()方法进行注入。

构造注入：调用构造方法进行注入。

**Spring 容器**：

Spring容器是生成Bean实例的工厂，如BeanFactory，ApplicationContext，XmlBeanFactory等。推荐使用ApplicationContext。

容器中bean的作用域：
- singleton：单例模式，在整个Spring IoC容器中，使用singleton定义的bean将只有一个实例；
- prototype：原型模式，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新实例；



### Spring AOP

AOP（面向切面编程），将交叉业务逻辑织入到主业务逻辑中。底层是使用动态代理模式实现。

### 代理模式

作用：增强被代理对象；织入交叉业务逻辑增强主业务逻辑。

#### 静态代理：

代理实现完全由程序员自己实现；只能针对特定的对象实现代理

实现步骤：

①定义主业务逻辑接口；

②实现主业务接口；

③代理类实现接口，增强主业务；

a）声明被代理对象：成员变量；

b）绑定被代理对象：有参构造器；

c）代理类增强主业务：方法扩展；

#### 动态代理：

可以代理任意对象；

##### 1.JDK动态代理实现：

InvocationHandler接口和proxy类；

[Object](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/Object.html) [invoke](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/reflect/InvocationHandler.html#invoke(java.lang.Object, java.lang.reflect.Method, java.lang.Object[]))([Object](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/Object.html) proxy, [Method](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/reflect/Method.html) method, [Object](mk:@MSITStore:C:\Users\admin\Desktop\JDK_API_1.6_zh_中文.CHM::/java/lang/Object.html)[] args)；

proxy：代理对象

method：扩展方法

args：扩展方法需要的参数

static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) 

loader：被代理对象的类加载器；

interfaces：被代理类实现的接口；

h：在代理类中所需要做的处理；被代理对象的处理器；

代理对象必须实现接口，代理的是接口；

实现步骤：

①定义主业务逻辑接口；

②实现主业务接口；

③编写针对代理对象的处理方式； 

implements InvocationHandler，实现invoke()；

使用步骤：

①创建代理对象实例

②创建代理的处理器对象并绑定被代理对象；

③创建代理对象（被代理对象的接口）

proxy.newProxyInstance()，返回值是接口对象；

④通过代理对象调用主业务逻辑；

##### 2.cglib动态代理实现：

优点：代理类不需要实现接口；运行速度快；

实现步骤：

①实现主业务逻辑

②代理类implements MethodInterceptor

增强器：Enhancer；

实现interceptor方法

methodProxy.invokeSuper(被代理对象，方法参数)；

使用步骤：

①生成代理对象；

②代理对象调用方法；

## SpringMVC

SpringMVC 的加载流程：

1. 客户端发送请求到 DispatcherServlet（中央调度器）。
2. DispatcherServlet 查询 HandlerMapping（处理器映射器），找到处理请求的 Controller（处理器）。
3. DispatcherServlet 将请求转发给 Controller，Controller 处理请求，返回 ModelAndView（实体与视图）。
4. DispatcherServlet 查询 ViewResolver（视图解析器），找到 ModelAndView 指定的视图，渲染显示到客户端。