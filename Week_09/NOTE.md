# 学习笔记

## 高级动态规划

### 动态规划 Dynamic Programming

1. "Simplifying a complicated problem by breaking it down into simpler sub-problems" (in a recursive manner)
2. Divide & Conquer + Optimal substructure (分治 + 最优子结构)
3. 顺推形式：动态递推

### DP 顺推模板

```python

function DP():

    dp = [][] # 二维情况

    for i = 0 .. M {
        for j = 0 .. N {
            dp[i][j] = _Function(dp[ii][jj]...)
        }
    }

    return dp[M][N];

```

### 关键点

动态规划 和 递归或者分治 没有根本上的区别（关键看有无最优的子结构）

* 拥有共性：找到重复子问题
* 差异性：最优子结构、中途可以淘汰次优解
  
### 状态转移方程

* 爬楼梯，递推公式： f(n) = f(n-1) + f(n-2), f(1) = 1, f(0) = 0
* 不同路径，递推公式：f(x,y) = f(x-1,y) + f(x, y-1)
* 打家劫舍，递推公式：
  * 方法1：dp[i] = max(dp[i-2] + nums[i], dp[i-1])
  * 方法2：
    * dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
    * dp[i][1] = dp[i-1][0] + nums[i];
* 最小路径和，递推公式：dp[i][j] = min(dp[i-1][j], dp[i][j-1] + A[i][j])
* 股票买卖
  * 初始状态：
    * dp[-1][k][0] = dp[i][0][0] = 0
    * dp[-1][k][1] = dp[i][0][1] = -infinity
  * 状态转移方程
    * dp[i][k][0] = max(dp[i-1][k][0], dp[i-1][k][1] + prices[i])
    * dp[i][k][1] = max(dp[i-1][k][1], dp[i-1][k-1][0] - prices[i])

## 字符串算法

## Homework

* https://github.com/WiFeng/leetcode/tree/master/first-unique-character-in-a-string
* https://github.com/WiFeng/leetcode/tree/master/reverse-string-ii
* https://github.com/WiFeng/leetcode/tree/master/longest-increasing-subsequence
