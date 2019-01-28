Java 规约：

抽象类：AbstractXxx，BaseXxx。

异常类：XxxException。

测试类：XxxTest。

枚举类：XxxEnum。

属性不要使用 isXxx 命名。

包名**单数**：dao，service，util。

long 型：2L（大写L）。

按功能建多个常量类。

使用4个空格，而非tab。

尽量不使用可变参数编程。

使用常量调用 equals() ：`"test".equals(object);` 

所有的相同类型的包装类对象之间值的比较，全部使用 equals 方法比较。

所有的 POJO 类属性必须使用包装数据类型。 

RPC 方法的返回值和参数必须使用包装数据类型

所有的局部变量使用基本数据类型。 

> 包装类没有默认值，null可以多表示一种状态。

POJO 类必须写 toString 方法

定义 DO/DTO/VO 等 POJO 类时，不要设定任何属性默认值。 

序列化类新增属性时，请不要修改 serialVersionUID 字段。

禁止在 POJO 类中，同时存在对应属性 xxx 的 isXxx()和 getXxx()方法。 

循环体内，字符串的连接方式，使用 StringBuilder 的 append 方法进行扩展。 

 ArrayList的subList返回的是 ArrayList 的内部类 SubList

array = list.toArray(array);  

