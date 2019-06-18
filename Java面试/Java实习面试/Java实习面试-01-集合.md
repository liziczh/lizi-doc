# 集合

集合的产生：为保存数目不确定的对象，解决数组长度固定的问题；

集合类：可以**存储任意类型的对象**，并且**长度可动态扩展**的类；

| 集合与数组比较 | 数组     | 集合     |
| -------------- | -------- | -------- |
| 长度           | 长度固定 | 动态扩展 |
| 元素类型       | 一种类型 | 任意类型 |

## 集合 API

Collection：

- List：ArrayList，Vector，LinkedList；
- Set：HashSet，TreeSet；

Map：HashTable，HashMap，TreeMap，LinkedHashMap；

## Collection

Collection - 最基本的集合接口，一个 Collection 存储一组 Object。

Collection 子接口：List 接口，Set 接口；

### List

List - 列表**【有序可重复】**；

> 有序指插入顺序；

#### ArrayList

ArrayList - 基于**可变数组**实现的 List；

- 允许多个 null 值（有序可重复）；
- 线程不同步（线程不安全）；
- 查询效率高；

#### Vector（已过时）

Vector - 基于**可变数组**实现的 List；

- 允许多个 null 值（有序可重复）；
- 线程同步（线程安全）；
- 查询效率高；

#### LinkedList

LinkedList - 基于**双向链表**实现的 List

- 允许多个 null 值（有序可重复）；
- 线程不同步（线程不安全）；
- 插入删除效率高；

### Set

Set - 数学意义上的集合**【无序不重复】**，满足确定性、互异性、无序性；

> 无序指插入顺序；

#### HashSet

HashSet - 基于 HashMap （哈希表）实现，元素为 HashMap 的 key；

- 允许一个 null 值（无序不重复）；
- 线程不同步（线程不安全）；
- 通过 equals() 和 hashCode() 方法判断重复元素。

**Java Object 重写 equals() 方法的同时为什么要重写 hashCode()？** 

因为 equals() 与 hashCode() 必须保持一致；

- 当 obj1.equals(obj2) 为 true，obj1.hashCode() 必须等于 obj2.hashCode()；
- 当obj1.hashCode() == obj2.hashCode()为false时，obj1.equals(obj2)必须为false；

#### TreeSet

TreeSet- 基于 TreeMap （二叉树）实现，元素为 TreeMap 的 key；主要用来**元素排序**。

- 允许一个 null 值（无序不重复）；
- 线程不同步（线程不安全）；
- 通过集合元素类实现 Comparable 接口，重写 compareTo() 方法判断重复元素。

##### 自然排序（Comparable）

自然排序：通过集合元素类实现 Comparable 接口，重写 compareTo() 方法排序。

comparaTo() 返回值：

TreeSet 底层为一个二叉树

- `return 0;` 表示集合中只存一个元素。元素值每次比较，都认为是相同的元素，这时就不再向TreeSet中插入除第一个外的新元素。

- `return 1;` 表示集合正序排列。元素值每次比较，都认为新插入的元素比上一个元素大，于是二叉树存储时，会存在根的右侧，读取时就是正序排列的。
- `return -1;` 表示集合倒序排列。元素值每次比较，都认为新插入的元素比上一个元素小，于是二叉树存储时，会存在根的左侧，读取时就是倒序序排列的。

##### 比较器排序 `Comparator<T>` 

创建 TreeSet 类时制定一个 Comparator 接口，重写 compara() 方法制定排序规则。

### Iterator 迭代器

hasNext() - 判断是否存在下一个元素；

next() - 获取下一个元素；



### ListIterator



## Map

Map - 键值对集合**【无序双列集合】**，一个 Map 存储一组键值对，提供键 ( key ) 与值 ( value ) 映射。

- 键不允许重复；

### HashTable

HashTable - 散列表，

- 线程同步（使用 synchronized 实现同步）

### HashMap ★

HashMap - 基于哈希桶数组实现的 Map；

- 生成相同 hashCode 的不同 key 存储在同一个 bucket 下，null key 存储在 0 bucket 下。

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



### TreeMap

TreeMap - 基于红黑树实现的 Map；



## Collections 工具类

