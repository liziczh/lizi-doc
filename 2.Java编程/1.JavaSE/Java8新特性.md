### Java8新特性

##### 接口扩展方法

```java
public interface Formula {
	double cal(int a);
	default double sqrt(int a){
		return  Math.sqrt(a);
	}
}
```