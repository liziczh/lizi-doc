# Java Interview Question

## Java

#### Java Object 重写 equals() 方法的同时为什么要重写 hashCode()？

因为 equals() 与 hashCode() 必须保持一致；
- 当 obj1.equals(obj2) 为 true，obj1.hashCode() 必须等于 obj2.hashCode()；
- 当obj1.hashCode() == obj2.hashCode()为false时，obj1.equals(obj2)必须为false；

### 集合

#### HashTable与HashMap的区别

**1. 关于null**：

- HashTable不支持 null-key 和 null-value。HashTable 遇到 null，抛出 NullPointerException。
- HashMap支持 null-key 和 null-value。HashMap 对 null 做了特殊处理，将 null 的 hashCode 值定为了 0，从而将其存放在哈希表的第0个 bucket 中。

**2. 扩容方式**：

- HashTable 默认初始化容量大小为11，之后每次扩充为原来的2n+1。
- HashMap默认初始化容量大小为16，之后每次扩充为原来的2倍。

  > 在取模计算时，如果模数是2的幂，那么我们可以直接使用位运算来得到结果，效率要大大高于做除法。所以从hash计算的效率上，又是HashMap更胜一筹。

**3. 线程安全**：

- HashTable 线程安全（同步）；
- HashMap 线程不安全（不同步）；

  > HashTable已经被淘汰了，如果你不需要线程安全，使用HashMap；如果你需要线程安全，使用ConcurrentHashMap；

**4. 数据结构**：

- HashTable 数组+链表
- HashMap 数组+链表/红黑树（JDK1.8）

#### ConcurrentHashMap和Hashtable的区别

- HashTable 采用 synchronized 实现同步，单锁锁定整个集合，迭代时其他线程必须等待其迭代完成才能访问 map，所以当 Hashtable 的大小增加到一定的时候，性能会急剧下降。
- ConcurrentHashMap 引入了分割（segmentation），仅锁定 map 的某个部分，更适用于高并发。



### 设计模式

#### 单例模式（Singleton）

懒汉式：

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

饿汉式：

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

#### 工厂模式



#### 代理模式



## 数据结构

线性表

链表

栈：先进后出

队列：先进先出

### 二叉树

二叉树（B 树）：有且仅有一个根节点，每个节点至多有两个子节点。



B+树：



红黑树（自平衡二叉查找树）：



哈夫曼树（最优树）：





### 查找

二分查找：



### 排序

**冒泡排序**：

```java
// 冒泡排序 ：大的沉底，小的上浮，相邻比较
public void bubbleSort(int[] a) {
    for (int i = a.length - 1; i > 0; i--) {
		for (int j = 0; j < i; j++) {
			if (a[j] > a[j + 1]) {
                int temp = a[j];
				a[j] = a[j+1];
                a[j+1] = temp;
			}
		}
	}
}
```

**选择排序**：

```java
// 选择排序：缩小范围，两两比较
public void selectSort(int[] arr) {
	for (int i = 0; i < arr.length - 1; i++) {
		for (int j = i + 1; j < arr.length; j++) {
			if (arr[i] > arr[j]) {
				int temp = a[i];
				a[i] = a[j];
                a[j] = temp;
			}
		}
	}
}
```

**插入排序**：

```java
// 插入排序：多次比较，一次插入
public void insertSort(int[] arr) {
	int j = 0;
	int key = 0; // 新牌
	for (int i = 1; i < arr.length; i++) {
		key = arr[i];// 抽取新牌
		for (j = i - 1; j >= 0 && key < arr[j]; j--) {
			arr[j + 1] = arr[j];
		}
		arr[j + 1] = key;// 已经j--，必须j+1
	}
}
```

**快速排序**：

```java
/*
 * 快速排序：交换排序 
 * pivot：枢轴指针; pivotkey：枢轴值，
 * low：低指针， high：高指针，
 */
// 分区函数，返回枢轴指针：partition(数组名称,起始位下标,末尾位下标); 
public int partition(int[] arr, int low, int high) {
	int pivotkey = arr[low];// 设第一个元素为枢轴值
	while (low < high) {
		while (low < high && arr[high] >= pivotkey) {
			high--;
		}
		arr[low] = arr[high];
		while (low < high && arr[low] <= pivotkey) {
			low++;
		}
		arr[high] = arr[low];
	}
	arr[low] = pivotkey;// 将枢轴值 给到 可覆盖区
	return low;// 返回枢轴指针
}
// 分区递归：qSort(数组名称,起始位下标,末尾位下标);
public void qSort(int[] arr, int low, int high) {
	if (low < high) {
		int pivot = partition(arr, low, high);// 枢轴指针
		qSort(arr, low, pivot - 1);// 前半部分
		qSort(arr, pivot + 1, high);// 后半部分
	}
}
// 快速排序
public void quickSort(int[] arr) {
	qSort(arr, 0, arr.length - 1);
}
```





## 数据库





## JavaWeb





## SSM 框架

### MyBatis



### Spring



### SpringMVC





