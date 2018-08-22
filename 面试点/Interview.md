# Interview Question



### JavaSE 集合

#### HashTable与HashMap的特点



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

