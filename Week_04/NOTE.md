# 学习笔记

## 深度优先搜索
  * Depth First Search
  * 可以使用递归实现，也可以通过自行维护栈来实现
  * 代码模板 - 递归写法
  ```
  visited = set()
  
  def dfs(node, visited):
      visited.add(node)
      #process current node here.
      ...
      for next_node in node.children():
          if not next_node in visited:
              dfs(next_node, visited)
  ```
## 广度优先搜索
  * Breadth First Search
  * 需要借助队列来实现（广度优先就不能通过递归实现了）
  * 代码模板
  ```
  def BFS(graph, start, end):
      queue = []
      queue.append([start])
      visited.add(start)
      
      while queue:
          node = queue.pop()
          visited.add(node)
          
          process(node)
          nodes = generate_related_nodes(node)
          queue.push(nodes)
          
      # other processing work
      ...
  ```

## 贪心算法
  * 概念：贪心算法是一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是全局最好或最优的算法。
  * 与动态规划的区别：贪心算法对每个字问题的解决方案都做出选择，不能回退。动态规划则保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。
  * 贪心发可以解决一些最优化问题，如：求图中的最小生成树，求哈夫曼编码等。然而对于工程和生活中的问题，贪心法一般不能得到我们所要求的答案。
  * 一旦一个问题可以通过贪心法来解决，那么贪心法一般是解决这个问题的最好办法。由于贪心法的高效性以及其所求得的答案比较接近最优结果，贪心法也可以用作辅助算法或者直接解决一些要求结果不特别精确的问题。
  * 适用场景：简单地说，问题能够分解成子问题来解决，子问题的最优解能递推到最终问题的最优解。这种子问题最优解称为最优子结构。
  * 注意事项
    * 在要求计算全局最优解时，难点时需要判断使用贪心法得到的局部最优解最终结果是否能得到全局最优解！！
    * 有时可能需要把数据稍微转化一下，才可以使用贪心算法。
    * 贪心算法计算顺序有时是从头开始，有时可使用从尾开始。这个思维非常重要。
## 二分查找
  * 前提条件
    * 目标函数单调性（单调递增或者递减）
    * 存在上下界（bounded）
    * 能够通过索引访问（index accessible）
  * 代码模板
  ```
  left, right = 0, len(array) - 1
  while left <= right:
      mid = (left + right) / 2
      if array[mid] == target:
          # find the target!!
          break or return result
      elif array[mid] < target:
          left = mid + 1
      else:
          right = mid - 1
  ```
  * 注意事项
    * 原理较简单，几乎每个人都可以想到。但是如果不进行多次重复，仍然容易无从下手，需要进行刻意联系，形成肌肉记忆~~
    * 边界处理需注意，在计算中值时，也需要关注对应编程语言除2后的取值情况。
    * 对应到通过索引访问这个点，因此链表就不可以直接使用二分查找。如果要对列表进行加速查找，则具有代表性的是跳表（skiplist）
    
# 五毒神掌
  每个题做5次及以上，形成肌肉式记忆
# 四部做题
 * 审题（输入、输出值范围，以及特殊值）
 * 分析可能的解法，及其时间、空间复杂度
 * 编码
 * 带入值分析，验证边界条件
 
# 个人感悟
  * 牛顿迭代法 没有看明白，抽时间再深入了解 （https://www.beyond3d.com/content/articles/8/）
  * 深度优先搜索，广度优先优先搜索本质上是对所有节点都遍历一次，并且仅且只访问一次。

# Homework
  * https://github.com/WiFeng/leetcode/tree/master/lemonade-change
  * https://github.com/WiFeng/leetcode/tree/master/best-time-to-buy-and-sell-stock-ii
  * https://github.com/WiFeng/leetcode/tree/master/assign-cookies
  * https://github.com/WiFeng/leetcode/tree/master/sqrtx
  * https://github.com/WiFeng/leetcode/tree/master/binary-search
  * https://github.com/WiFeng/leetcode/tree/master/search-in-rotated-sorted-array
         
