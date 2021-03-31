#### Java基础

##### Java跨平台原理

不同OS有不同的JVM，可以将一份.class文件执行为不同的机器码。

##### JDK、JRE、JVM

JDK（Java Development Kit）：Java开发工具包，包括JRE、开发工具javac/java/javap等。

JRE（Java Runtime Environment）：Java运行时环境，包括JVM，Java核心类库，所有的Java程序都要在JRE下才能运行。

JVM（Java Virtual Mechinal）：Java虚拟机，模拟计算机。

##### 基本数据类型

byte（1byte），short（2byte），int（4byte），long（8byte），

float（4byte），double（8byte），char（2byte），boolean（1bit）。

1byte=8bit；

类型提升：

① byte，short，char之间不会相互转换，在计算时会自动转换为int类型进行计算。

② Java运算时，自动提升至最大数据类型。

③ 浮点型运算结果是double。

##### ==与equals()的区别

==基本类型比较值；引用类型比较引用地址；

equals()本质也是==，只是很多类重写了Object的equals()方法将引用比较改为了值比较，类似String，Integer。

##### hashCode()与equals()的关系

equals()相同则hashCode()一定相同，hashCode()相同不一定equals()相同，因为**hash冲突**，不同的对象的哈希值可能相同。

重写equals()必须重写hashCode()，使对象间equals()比较与hashCode()比较保持一致。

补充说明：集合hash类存储结构的重复元素校验规则是直接hash寻址，先判断hashCode是否相等，再判断equals是否相等，提高插入时比较元素重复的效率。

##### &与&&

&位运算：前后都执行。

&&短路与：前为false后不执行。

##### 访问控制

private：类内访问。

default：包内访问。

protected：子类访问。

public：跨包访问。