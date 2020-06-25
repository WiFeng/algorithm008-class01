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

### 字符串

* python / java 字符串是不可变的 ，immutable。即，改变字符串内容时，实际上是创建一个新的字符串。同时也是线程安全的。
* C++ 是可变的。
* 参考地址： https://lemire.me/blog/2017/07/07/are-your-strings-immutable/

### 遍历字符串

```python
for ch in "abbc" :
    print(ch)
```

```java
String x = "abbc";
for (int i = 0; i < x.size(); ++i) {
    char ch = x.charAt(i);
}
for ch in x.toCharArray() {
    System.out.println(ch);
}
```

```C++
string x("abbc");
for (int i = 0; i < s1.length(); i++) {
    cout << x[i];
}
```

### 字符串比较

```Java
String x = "abc";
String y = "abb";

x == y --> false

x.equals(y) --> true
x.equalsIgnoreCase(y) --> true
```

### 高级字符串算法

### 最长子串、子序列

1. Longest common sequence (最长子序列)
   
   dp[i][j] = dp[i-1][j-1] + 1 (if s1[i-1] == s2[j-1])
   else dp[i][j] = max(dp[i-1][j], dp[i][j-1])

2. Longest common substring（最长子串）
   
   dp[i][j] = dp[i-1][j-1] + 1 (if s1[i-1] == s2[j-1])
   else dp[i][j] = 0

3. Edit Distance（编辑距离）

### 字符串匹配算法

1. 暴力法（brute force）
2. Rabin-Karp 算法
3. KMP 算法

参考链接：
* Boyer-Moore 算法：https://www.ruanyifeng.com/blog/2013/05/boyer-moore_string_search_algorithm.html
* Sunday 算法：https://blog.csdn.net/u012505432/article/details/52210975

#### 暴力法

```java
public static int forceSearch(String txt, String pat) {
    int M = txt.length();
    int N = pat.length();
    for (int i = 0; i <= M-N; i++) {
        int j;
        for (j = 0; j < N; j++) {
            if (txt.charAt(i+j) != pat.charAt(j))
                break;
        }
        if (j == N) {
            return i;
        }
    }
    return -1;
}
```

#### Rabin-Karp 算法

在朴素算法中，我们需要挨个比较所有字符，才知道目标字符串中是否包含子串。那么，是否有别的方法可以用来判断目标字符串是否包含子串呢？

答案是肯定的，确实存在一种更快的方法。为了避免挨个字符对目标字符串和子串进行比较，我们可以尝试一次性判断两者是否相等。因此，我们需要一个好的哈希函数（hash function）。通过哈希函数，我们可以算出子串的哈希值，然后将它和目标字符串中的子串的哈希值进行比较。这个新方法在速度上比暴力法有限制提升。

Rabin-Karp 算法的思想：

1. 假设子串的长度为M(pat)，目标字符串的长度为N(txt)
2. 计算子串的hash值 hash_pat
3. 计算目标字符串txt中每个长度为 M 的子串的 hash 值（共需要计算 N-M+1次）
4. 比较 hash 值：如果 hash 值不同，字符串必然不匹配；如果 hash 值相同，还需要使用朴素算法再次判断

```java
public final static int D = 256;
public final static int Q = 9997;

static int RabinKarpSearch(String txt, String pat) {
    int M = pat.length();
    int N = txt.length();
    int i, j;
    int patHash = 0, txtHash = 0;

    for (i = 0; i < M; i++) {
        patHash = (D * patHash + pat.charAt(i)) % Q;
        txtHash = (D * txtHash + pat.charAt(i)) % Q;
    }

    int highestPow = 1; // pow(256, M-1)
    for (i = 0; i < M -1; i++)
        highestPow = (highestPow * D) % Q;

    for (i = 0; i <= N - M; i++) {
        if (patHash == txtHash) {
            for (j = 0; j < M; j++) {
                if (txt.charAt(i+j) != pat.charAt(j))
                    break;
            }
            if (j == M)
                return i;
        }
        if (i < N - M) {
            txtHash = (D * (txtHash - txt.charAt(i) * highestPow) + txt.charAt(i + M)) % Q;
            if (txtHash < 0)
                txtHashh += Q;
        }
    }
    return -1;
}
```

#### KMP 算法

KMP 算法 （Knuth-Morris-Pratt）的思想就是，当子串与目标字符串不匹配时，其实你已经知道了前面已经匹配成功那一部分的字符（包括子串与目标字符串）。以阮一峰的文章为例，当空格与D不匹配时，你其实知道前面六个字符是“ABCDAB”。KMP 算法的想法是，设法利用这个已知信息，不要把“搜索位置”移回已经比较过的位置，继续把它项后移，这样就提高了效率。

* https://www.bilibili.com/video/av11866460?from=search&seid=17425875345653862171
* http://www.ruanyifeng.com/blog/2013/05/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm.html
  
## Homework

* https://github.com/WiFeng/leetcode/tree/master/first-unique-character-in-a-string
* https://github.com/WiFeng/leetcode/tree/master/reverse-string-ii
* https://github.com/WiFeng/leetcode/tree/master/longest-increasing-subsequence
