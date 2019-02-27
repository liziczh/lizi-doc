# Java Lambda

Lambda表达式

```java
(parameters) -> expression
(param) -> {statements;}
```

集合操作：

```java
itemList.forEach((item) -> System.out::println);
```

## Java函数式接口

### Consumer

`accept()`：有输入无输出。

`andThen()`：后调。

### Function

`apply()`：有输入有输出。

`compose()`：

`andThen()`：

`indentity()`：

### Predicate

Predicate：判断

`and()`：与

`or()`：或

`negate()`：非

## Java函数式编程接口

### Stream

stream不保存数据，它是有关算法和计算的，更像是个高级版本的Iterator。

map()：映射

limit()：取前n个

forEach()：

collect(toList());

get();