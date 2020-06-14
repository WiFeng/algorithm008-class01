# 学习笔记

## 感触总结

* 人肉递归很累
* 找到最近最简方法，将其拆解为可重复解决的问题
* 数学归纳法的思维（抵制人肉递归的诱惑）

## 递归思维理解

通过画递归树的形式，理解斐波拉契数列状态树、重复子状态。 比如求Fib(6)，人脑记忆不那么准确，因此用笔在纸上把递归树一个一个画出来即可。这时你会发现你虽然只要计算第六个数，但是你的状态树可不是六个，它是2的N次方。这时你会发现它的节点的扩散是指数级的。通过这个过程，你会对一个问题它的状态树在脑子里面，有一个树形结合的概念，这样对于初学者来说更方便大家理解。

## 动态规划

### 基本概念

* wiki 定义 （Dynamic Programming）
* "Simplifying a complicated problem by breaking it down into simpler sub-problems" (in a recursive manner)
* Divide & Conquer + Optimal substructure (分治+最优子结构)

### 关键点

* 动态规划和递归或者分治没有根本上的区别（关键看有无最优子结构）
* 共性：找到重复子问题
* 差异性：最优子结构、中途可以淘汰次优解
  
## Homework

* https://github.com/WiFeng/leetcode/tree/master/fibonacci-number
* https://github.com/WiFeng/leetcode/tree/master/unique-paths