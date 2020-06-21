# 学习笔记

## 位运算

为什么需要位运算符

* 机器里的数字表示方式和存储格式就是二进制
* 十进制 <-> 二进制： 如何转换？ https://zh.wikihow.com/%E4%BB%8E%E5%8D%81%E8%BF%9B%E5%88%B6%E8%BD%AC%E6%8D%A2%E4%B8%BA%E4%BA%8C%E8%BF%9B%E5%88%B6

### 位运算符

* 左移 <<
* 右移 >>
* 按位或 |
* 按位与 &
* 按位取反 ~
* 按位异或（相同为零不同为一）^

#### XOR 异或

* x ^ 0 = x
* x ^ 1s = ~x // 注意 1s = ~0
* x ^ (~x) = 1s
* x ^ x = 0
* c = a ^ b => a ^ c = b, b ^ c = a // 交换2个数
* a ^ b ^ c = a ^ (b ^ c) = (a ^ b) ^ c // associcative

#### 指定位置的位运算

1. 将 x 最右边的 n 位清零： x & (~0 << n)
2. 获取 x 的第 n 位值（0或者1）： （x >> n） & 1
3. 获取 x 的第 n 位的幂值：x & (1 << n)
4. 仅将第n位置为1：x | (1 << n)
5. 仅将第n位置为0：x & (~(1 << n))
6. 将 x 最高位至第 n 位（含）清零：x & ((1 << n) - 1)

#### 实战位运算要点

* 判断奇偶：
  * x % 2 == 1 --> (x&1) == 1
  * x % 2 == 0 --> (x&1) == 0
* x >> 1 --> x / 2
  * 即 x = x / 2; x = x >> 1;
  * mid = (left+right)/2; --> mid = (left+right) >> 1;
* x = x & (x - 1) 清零最低位的1
* x & -x => 得到最低位的1 ？？？
* x & ~x => 0

### 算术移位与逻辑移位

### 位运算的应用

* 布隆过滤器
* N皇后问题
  
## 布隆过滤器

### Bloom Filter vs Hash Table

一个很长的二进制向量和一系列随机映射函数。布隆过滤器可以用于检索一个元素是否在一个集合中。

* 优点：空间效率和查询时间都远远超过一般的算法。
* 缺点：有一定的误识别率和删除困难。

### 布隆过滤器的特点

* 如果查询一个元素在，结果：可能在，也可能不在。
* 如果查询一个元素不在，结果：一定不在。

### 布隆过滤器案例

* 比特币网络
* 分布式系统（Map-Reduce）Hadoop 、 search engine
* Redis 缓存
* 垃圾邮件、评论等的过滤

### 布隆过滤器实现

```python

from bitarray import bitarray
import mmh3

class BloomFilter:
    def __init__(self, size, hash_num):
        self.size = size
        self.hash_num = hash_num
        self.bit_array = bitarray(size)
        self.bit_array.setall(0)

    def add(self, s):
        for seed in range(self.hash_num):
            result = mmh3.hash(s, seed) % self.size
            self.bit_array[result] = 1

    def lookup(self, s):
        for seed in range(self.hash_num):
            result = mmh3.hash(s, seed) % self.size
            if self.bit_array[result] == 0
                return "Nope"
        return "Probably"

bf = BloomFilter(500000, 7)
bf.add("dantezhao")
print(bf.loopup("dantezhao"))
print(bf.loopup("yyj"))

```

## LRU Cache

### cache 缓存

### CPU socket

* L1 cache
* L2 cache
* L3 cache

### LRU Cache 基本特性

* 两个要素：大小、替换策略
* Hash Table + Doulbe LinkedList
* 查询 O(1)
* 修改、更新 O(1)

### 替换策略

* LFU - least frequently used
* LRU - least recently used

替换算法总览：
https://en.wikipedia.org/wiki/Cache_replacement_policies

## 排序算法

1. 比较排序：
   通过比较来决定元素间的相对次序，由于其时间复杂度不能突破O(nlogn)，因此也称为非线性时间比较类排序。
   * 交换排序
     * 冒泡排序
     * 快速排序
   * 插入排序
     * 简单插入排序
     * 希尔排序
   * 选择排序
     * 简单选择排序
     * 堆排序
   * 归并排序
     * 二路归并排序
     * 多路归并排序

2. 非比较类排序：
   不通过比较来决定元素间的相对次序，它可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序。
   * 计数排序
   * 桶排序
   * 基数排序

### 初级排序 - O(n^2)

1. 选择排序（Selection Sort）
   每次找最小值，然后放到待排序数组的起始位置
2. 插入排序（Insertion Sort）
   从前到后逐步构建有序序列；对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。
3. 冒泡排序（Bubble Sort）
   嵌套循环，每次查看相邻的元素，如果逆序则交换。

### 高级排序 - O(N * LogN)

#### 快速排序（Quick Sort）

* 数组取标杆 pivot，将小元素放 pivot 左边，大元素放右侧，然后依次对右边和右边的子数组继续快排；以达到整个序列有序。
  
快排代码 - Java

```Java

    public static void quickSort(int[] array, int begin, int end) {
        if (end <= begin) return;
        int pivot = partition(array, begin, end);
        quickSort(array, begin, privot - 1);
        quickSort(array, privot + 1, end);
    }

    static int partition(int[] a, int begin, int end) {
        // pivot: 标杆位置，counter：小于pivot的元素的个数
        int pivot = end, count = begin;
        for (int i = begin; i < end; i++) {
            if (a[i] < a[pivot]) {
                int temp = a[counter]; 
                a[counter] = a[i];
                a[i] = temp;
                counter++;
            }
        }
        int temp = a[pivot];
        a[pivot] = a[counter];
        a[counter] = temp;
        return counter;
    }

    // 调用方式：quickSort(q, 0, a.length - 1)

```

#### 归并排序（merge Sort）- 分治

1. 把长度为 n 的输入序列分成两个长度为 n/2 的子序列；
2. 对这两个子序列分别采用归并排序；
3. 将两个排序好的子序列合并成一个最终的排序序列。

归并排序代码 - java

```java

public static void mergeSort(int[] array, int left, int right) {

    if (right <= left) return;
    int mid = (left + right) >> 1; // (left + right) / 2;

    mergeSort(array, left, mid);
    mergeSort(array, mid + 1, right);
    merge(array, left, mid, right);
}

public static void merge(int[] arr, int left, int mid, int right) {

    int[] temp = new int[right - left + 1]; // 中间数组
    int i = left, j = mid + 1, k = 0;

    while (i <= mid && j <= right) {
        temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    }

    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];

    for (int p = 0; i < temp.length; p++) {
        arr[left + p] = temp[p];
    }
    // 也可以用 Systemp.arraycopy(a, start1, b, start2, length)
}

```

归并 和 快排 具有相似性，但步骤顺序相反

* 归并：先排序左右子数组，然后合并两个有序子数组
* 快排：先调配出左右子数组，然后对于左右子数组进行排序

#### 堆排序 （Heap Sort） - 堆插入 O(logN)，取最小、大值O(1)

1. 数组元素依次建立小顶堆
2. 依次取堆顶元素，并删除

堆排序代码 - C++

```C++

void heap_sort(int a[], int len) {

    priority_queue<int, vector<int>, greater<int>> q;

    for (int i = 0; i < len; i++) {
        q.push(a[i]);
    }

    for (int i = 0; i < len; i++) {
        a[i] = q.pop();
    }
}

```

```C++

static void heapify(int[] array, int length, int i) {
    int left = 2 * i + 1, right = 2 * i + 2;
    int largest = i;

    if (left < length && array[left] > array[largest]) {
        largest = leftChild;
    }
    if (right < length && array[right] > array[largest]) {
        largest = right;
    }

    if (largest != i) {
        int temp = array[i];
        array[i] = array[largest];
        array[largest] = temp;
        heapify(array, length, largest);
    }
}

public static void heapSort(int[] array) {
    if (array.length == 0) return;

    int length = array.length;
    for (int i = length / 2 - 1; i >= 0; i--) {
        heapify(array, length, i);
    }

    for (int i = length - 1; i >= 0; i--) {
        int temp = array[0];
        array[0] = array[i];
        array[i] = temp;
        heapify(array, i, 0);
    }
}

```

### 特殊排序

* 计数排序（counting sort）
  计数排序要求输入的数据必须是有确定范围的整数。将输入的数据值转化为键存储在额外开辟的数组空间中；然后依次把计数大于 1 的填充回原数组

* 桶排序（Bucket Sort）
  桶排序（Bucket Sort）的工作原理：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶在分别排序（有可能再使用别的排序算法或者是以递归方式继续使用桶排序进行排）。

* 基数排序（Radix Sort）
  基数排序是按照低位先排序，然后收集；在按照高位排序，然后再收集；依次类推，知道最高位。有时候有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序。

### 排序动画

* https://www.cnblogs.com/onepixel/p/7674659.html
* https://www.bilibili.com/video/av25136272
* https://www.bilibili.com/video/av63851336

## Homework

* https://github.com/WiFeng/leetcode/tree/master/number-of-1-bits
* https://github.com/WiFeng/leetcode/tree/master/power-of-two
* https://github.com/WiFeng/leetcode/tree/master/lru-cache