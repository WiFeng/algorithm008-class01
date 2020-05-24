# 学习笔记

## 泛型递归、树的递归

* 树相关的面试题为什么总是使用递归？
  * 节点的定义（树的定义本身就是一种递归的形式）
  * 重复性（自相似性）
* 递归(Recursion)
  * 通过函数体来进行的循环
  * 例如：盗墓空间、从前有个庙，庙里有个小和尚的故事。有一种归去来兮的感觉。
* 递归示例(计算n!)

    ``` python
    def Factorial(n):
        if n <= 1:
            return 1
        return n * Factorial(n - 1)
    ```

* 递归模板

    ``` python
    def recursion(level, param1, param2, ...):
        # recursion terminator
        if level > MAX_LEVEL:
            process_result
            return

        # process logic in current level
        process(level, data...)

        # drill down
        self.recursion(level+1, p1, ...)

        # reverse the current level status if needed
    ```
  
* 思维要点
  * 不要人肉进行递归（最大的误区）
  * 找到最近最简方法，将其拆解成可重复解决的子问题（重复子问题）
  * 数学归纳法思维

## 分治

* 概念：分治是一种特殊的、较为复杂的递归。
* 分治代码模板
  
  ``` python
    def divide_conquer(problem, param1, param2, ...):
        # recursion teminator
        if problem is None:
            print_result
            return

        # prepare data
        data = prepare_data(problem)
        subproblems = split_problem(problem, data)

        # conquer subproblems
        subresult1 = self.divide_conquer(subproblem[0], p1, ...)
        subresult2 = self.divide_conquer(subproblem[1], p2, ...)
        subresult3 = self.divide_conquer(subproblem[2], p3, ...)

        # process and generate the final result
        result = process_result(subresult1, subresult2, subresult3, ...)

        # revert the current level states
  ```

* 关键要点
  * 给定义个现实中的问题或者面试题目，怎么样把它拆分成子问题是比较重要的，主要看经验，也是最有难度的，也就是CEO，或者架构师所做的事情。
  * 怎么样merge子结果也是比较关键。也就是说，当得到几个子结果，把他们合并起来也有个讲究。
  * 当前层处理当前层的问题，一般不要下探，或者不要下探太多层。如果公司管理者，事无巨细的管理是非常糟糕的。
  
## 回溯

* 定义
  回溯法采用试错的思想，它尝试分布去解决一个问题。在分布解决问题的过程中，当它通过尝试发现有的分布答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其他可能的分布解答再次尝试寻找问题的答案。

  回溯法通常用最简单的递归方法来实现，在反复重复上述步骤后可能出现两种情况：
  * 找到一个可能存在的正确的答案；
  * 在尝试了所有可能的分布方法后宣告该问题没有答案。
  
  在最坏的情况下，回溯法会导致一次复杂度为指数时间的计算。

## 个人感悟

分治相对比较好理解及实现，但是回溯还有有点绕，大脑完全跟踪不过来。

特别是在理解排列组合题目时，看了官方题解，代码非常简单。看代码之前虽然知道了分治回溯的思路，但是依然无从下手。看代码之后，依然对于回溯链路理解不是非常透彻。

## homework

* https://github.com/WiFeng/leetcode/tree/master/powx-n
* https://github.com/WiFeng/leetcode/tree/master/permutations