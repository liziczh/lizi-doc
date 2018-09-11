# 设计模式

### 单例模式（Singleton）

**懒汉式**：

```java
public class Singletonif {
	// 静态实例化
	private static Singletonif s = null;
	// 私有化构造方法
	private Singletonif() {
	}
	// 公开提供静态获取实例的方法
	public static Singletonif getInstance() {
		if (s == null) {
			s = new Singletonif();
		}
		return s;
	}
}
```

**饿汉式**：

```java
public class Singletonfinal {
	// 静态 final 实例化
	private static final Singletonfinal s = new Singletonfinal();
	// 私有化构造方法
	private Singletonfinal() {
	}
	// 公开提供静态获取实例的方法
	public static Singletonfinal getInstance() {
		return s;
	}
}
```

### 工厂模式

工厂模式：实例化对象模式；（工厂代替new操作）；

#### 简单工厂模式

静态工厂模式；

①具体工厂角色：创建产品对象

②抽象产品角色：定义产品的标准和规范

③具体产品角色：具体实现产品

优点：创建对象不使用new，使用工厂创建；

缺点：使工厂与产品产生了高度耦合，不符合类设计的开闭原则；

#### 工厂方法模式

①抽象工厂角色：

②具体工厂角色：

③抽象产品角色：

④具体产品角色：

优点：抽象工厂类的存在，降低了工厂与产品的耦合度；

符合程序的开闭原则，程序扩展性更强；

缺点：在扩展程序的时候，都需要创建具体工厂，程序较复杂；

#### 抽象工厂模式

①抽象工厂角色：

②具体工厂角色：

③抽象产品角色：

④具体产品角色：

优点：分离接口与实现：客户端使用抽象工厂来创建需要的对象；

切换产品族变的容易：

缺点：不易扩展新产品：

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

 