# 学习笔记

## 哈希表

    * a. 通常查询、插入 时间复杂度均为 O(1)
  
    * b. 通过哈希函数把插入的键转换为一个数字，然后根据哈希表的大小求模计算出对应存放位置
  
    * c. 哈希函数并不能保证结果的唯一性，因此要考虑哈希冲突的情况，通常在一个位置（槽）上维护一个链表来解决。当然，每个链表的节点需要保持完成的键与值（正常虽然是根据键来查找值，可能感觉不不要保持键值，但是为了在冲突的情况下能够准确识别出对应的键，需要节点上记录键值）
  
    * d. 当哈希表中的元素越来越多时，以至于超过哈希表大小时，冲突的概率就会比较高。这时我们需要扩大哈希表的大小，同时也就需要重新对其中已存在的元素进行哈希计算，存放到新的位置。这个开销还是比较大，所以每次扩大时，可以扩大为原尺寸的2倍，预留一些空间，避免这种操作频繁进行。
  
## 树、二叉树、二叉搜索树

    * a. 树是一种有根节点，对应子节点，子节点再对应子节点的结构，类似于公司组织架构，家庭成员关系的结构。也其实是对数据进行了分层、分类，进而达到加速查找的目的。
    * b. 二叉树是最简单的一种数。二叉树，对整颗数遍历的时间复杂度为O(n)，分为前序、中序、后序遍历。中序：左子，父，右子。前序：父，左子，右子。右序：左子，右子，父。
    * c. 二叉搜索树，顾名思义是一种有序的的二叉树，因此可以加速查找。特性为：左子节点 < 父节点 < 右子节点，其中一个节点的值应该大于左子树上所有节点，小于右子树上所有的节点。

## 堆、二叉堆

    * a. 堆是一种针对提取最大值、或者最小值的数据结构。有多种实现方式，其中斐波拉契堆、严格的斐波拉契堆实现方式效果最好，插入达到O(1)的时间复杂度，但是实现比较复杂。二叉堆实现方式较容易理解，所以初学者也常常通过学习二叉堆来理解堆的特性。
  
    * b. 二叉堆是完整二叉树，因此父节点与子节点的位置是相对固定，也就不需要使用链表，而直接可以使用连续的内存空间，通过索引来操作（有时可能会说这种方式为数组形式，但严格来讲，这种形式具备数组形式的特性，但并不是C语言中的数组，因为C语言中的数组定义是需要指定数组长度，并且不能使用变量）。可以相互计算出来的。如果把根节点的位置按照 0 开始计算，那么其中节点i的左子节点为：2i+1，右子节点为：2i+2。其中节点 i 的父父节点为 floor((i-1)/2)。

## 总结

本周算法习题只完成了一道，本来除工作之外闲暇时间并不多，但是这个题还花费了非常大的时间，但是感觉收获还是比较多的。其中涵盖了哈希表、二叉堆，完全自己实现，并未使用类库：

1. https://github.com/WiFeng/leetcode/tree/master/top-k-frequent-elements

补之前未完成的算法题：

1. https://github.com/WiFeng/leetcode/tree/master/largest-rectangle-in-histogram
