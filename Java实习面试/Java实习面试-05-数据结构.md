# 数据结构

## 线性表

顺序表

单链表

双向链表

循环链表

## 栈与队列

栈：先进后出

队列：先进先出

## 树

二叉树：有且仅有一个根节点，每个节点至多有两个子节点。

### B+树

B+树：MySQL 索引

### 红黑树

红黑树（自平衡二叉查找树）

**红黑树特性**：
（1）每个节点要么红色，要么黑色。
（2）根节点是黑色。
（3）每个叶子节点（NIL）都是黑色空节点。 [注意：这里叶子节点，是指为空(NIL或NULL)的叶子节点！]
（4）每个红色节点的两个子节点都是黑色。(从每个叶子到根的所有路径上不能有两个连续的红色节点)
（5）从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。

**变色**：红色节点变为黑色节点



**旋转**：

左旋转：

右旋转：





哈夫曼树（最优树）

 

## 图



## 查找

**二分查找**：

```java
// 二分查找
public static int binarySearch(int[] a, int key){
    int middle = a.length/2;
    int low = 0;
    int high = a.length - 1;
    while(low<=high){
        middle = (low+high)/2;
        if(a[middle] > key){
            high = middle -1;
        }else if(a[middle] <key){
            low = middle + 1;
        }else {
            return middle; // 返回目标值索引
        }
    }
    return -1; // 未查找到key
}
```

## 排序

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

