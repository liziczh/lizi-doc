### Java8

##### Lambda

```java
(param) -> expression
(param) -> {statement;}
```

##### 方法引用

方法引用：`::` 。

**构造器引用**：

- 范式：`Class<T>::new` ；

- 示例：`Car::new` ；

**静态方法引用**：

- 范式：`Class::staticMethod` ；
- 示例：`Car::create`；

**特定类的任意对象的方法引用**：

- 范式：`Class::method` ；
- 示例：`Car::repair`；

**特定对象的方法引用**：

- 范式：`instance::method`；
- 示例：`car::follow`；

##### Stream

 **stream()** ： 串行流。

 **parallelStream()** ：并行流。

forEach：迭代数据

map：映射每个元素对应结果

filter：根据条件过滤元素



##### Optional

