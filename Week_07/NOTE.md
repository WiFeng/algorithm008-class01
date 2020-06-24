# 学习笔记

## Trie 树

### 基本结构

* 字典树，即Trie树，又称单词查找树或键树，是一种树形结构。典型应用是用于统计和排序大量的字符串（但不限于字符串），所以经常被搜索引擎系统用于文本词频统计。
* 它的优点是：最大限度地减少无谓字符串比较，查询效率比哈希表高。

### 基本性质

1. 节点本身不存在完整单词
2. 从根节点到某一节点，路径上经过的字符串连接起来，为该节点对应的字符串；
3. 每个节点的所有子节点路径代表的字符都不相同。

### 核心思想

* Trie 树的核心思想是空间换时间。
* 利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

### Trie 代码模板

```python

class Trie(object):

    def __init__(self):
        self.root = {}
        self.end_of_word = "#"

    def insert(self, word):
        node = self.root
        for char in word:
            node = node.setdefault(char, {})
        node[self.end_of_word] = self.end_of_word

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node:
                return False
            node = node[char]
        return self.end_of_word in node

    def startsWith(self, prefix):
        node = self.root
        for char in word:
            if char not in node:
                return False
            node = node[char]
        return True
```

## 并查集 Disjoint Set

### 适用场景

* 组团、配对问题
* Group or not ?

### 基本操作

* makeSet(s): 建立一个新的并查集，其中包含s个单元素集合。
* unionSet(x, y): 把元素x和元素y所在的集合合并，要求x和y所在的集合不相交，如果相交则不合并。
* find(x): 找到元素x所在的集合的代表，该操作也可以用于判断两个元素是否位于同一个集合，只要将它们各自的代表比较一下就可以了。
  
* 初始化，自己指向自己
* 查询、合并，找出每个集合中的领头元素，然后把其中一个集合的领头元素的parent指向另一个集合的领头元素
* 路径压缩，把一层一层的关系，可以转换为把这些所有元素的parent指向同一个元素，这样在查询是就会方便很多

### 并查集 代码模板

```java
class UnionFind{
    private int count = 0;
    private int[] parent;

    public UnionFind(int n) {
        count = n;
        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }

    public int find(int p) {
        while(p != parent[p]) {
            parent[p] = parent[parent[p]];
            p = parent[p];
        }
        return p;
    }

    public void union(int p, int q) {
        int rootP = find(p);
        int rootQ = find(q);
        if (rootP == rootQ) return;
        parent[rootP] = rootQ;
        count--;
    }
}
```

```python
def init(p):
    # for i = 0 .. n: p[i] = i;
    p = [i for i range(n)]

def union(self, p, i, j):
    p1 = self.parent(p, i)
    p2 = self.parent(p, j)
    p[p1] = p2

def parent(self, p, i):
    root = i
    while p[root] != root:
        root  = p[root]
    while p[i] != i: # 路径压缩？
        x = i; i = p[i]; p[x] = root
    return root
```

### 感悟

* 至于这个算法结构是怎么想到的，这个问题就非常难了。
* 第一遍有点懵，第二遍好像是那么回事，第三遍有种豁然开朗的感觉。（现在还只是到第二遍）
* 这个算法，会就是会，不会就是不会，没有太多的需要随机应变的情况，一般适合的题目都能够很好的识别
* 学懂这个算法，对于算法的学习可以进阶到更好的一个台阶，可见它的重要性。

## 高级搜索

### 初级搜索

1. 朴素搜索
2. 优化方式：不重复（fibonacci）、剪枝（生成括号问题）
3. 搜索方向：
    * DFS: depth first search 深度优先搜索
    * BFS: breadth first search 广度优先搜索
    * 双向搜索、启发式搜素
  
### DFS 代码 - 递归写法

```python

visited = set()

def dfs(node, visited):
    if node in visited: # terminator
        # already visited
        return

    visited.add(node)

    # process current node here.
    ...
    for next_node in node.children():
        if not next_node in visited:
            dfs(next_node, visited)

```

### DFS 代码 - 非递归写法

```python

def DFS(self, tree):

    if tree.root is None:
        return []

    visited, stack = [], [tree.root]

    while stack:
        node  = stack.pop()
        visited.add(node)

        process(node)
        nodes = generate_related_node(node)
        stack.push(nodes)

    # other processing work

```

### BFS 代码

```python

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

```

### 回溯法

回溯法采用试错的思想，它尝试分布去解决一个问题。在分布解决问题的过程中，当它通过尝试发现现有的分布答案不能得到有效的正确的解答的时候，它将取消上一步甚至是上几步的计算，再通过其他的可能的分布解答再次尝试寻找问题的答案。

回溯法通过用最简单的递归方法来实现，在反复重复上述的步骤后可能出现两种情况：

* 找到一个可能存在的正确的答案
* 在尝试了所有可能的分布方法后宣告该问题没有答案

在最坏的情况下，回溯法会导致一次复杂度为指数时间的计算。

### 剪枝

* 实现和特性

### 双向BFS

* 实现和特性

### 启发式搜素

* 实现和特性 , A* search

```python
def AstarSearch(graph, start, end):
    pq = collection.priority_queue() # 优先级 -> 估价函数
    pq.append([start])
    visited.add(start)

    while pq:
        node = qp.pop() # can we add more intelligence here ?
        visited.add(node)

        process(node)
        nodes = generate_related_nodes(node)
        unvisited = [node for node in nodes if node not in visited]
        pq.push(unvisited)
```

#### 估价函数

启发式函数：h(n)，它用来评价哪些节点最优希望的是一个我们要找的节点，h(n)会返回一个非负实数，也可以认为是从节点n的目标节点路径的估价成本。

启发式函数是一种告知搜搜方向的方法。它提供了一种明智的方法来猜测哪个邻居节点会导向一个目标。

## 红黑树 、 AVL树

### 回顾二叉树

* 树 -> 二叉树 -> 二叉搜索树 -> 平衡二叉搜素树
* 三种遍历顺序(三种方式左节点与右节点次序都没变，变化的是根节点的位置)
    1. 前序（pre-order）: 根-左-右
    2. 中序（in-order）: 左-根-右
    3. 后序（post-order）: 左右根
* 示例代码

```python
def preorder(self, root):
    if root:
        self.traverse_path.append(root.val)
        self.preorder(root.left)
        self.preorder(root.right)

def inorder(self, root):
    if root:
        self.inorder(root.left)
        self.traverse_path.append(root.val)
        self.inorder(root.right)

def postorder(self, root):
    if root:
        self.postorder(root.left)
        self.postorder(root.right)
        self.traverse_path.append(root.val)

```

* 树跟链表没有本质上的区别，当一个链表分出2个next的话，我们就把它称之为树。本质上，是从一维空间扩散到了二维空间，这就引入了二叉搜索树。

#### 二叉搜素树 Binary Search Tree

二叉搜索树，也称二叉搜索树，有序二叉树（Ordered Binary Tree）、排序二叉树（Sorted Bianry Tree），是指一颗空树或者具有下列性质的二叉树：

* 左子树上所有节点的值均大于它的根节点的值；
* 右子树上所有节点的值均大于它的根节点的值；
* 以此类推：左、右子树也分别为二叉查找树。（这就是重复性！）
  
中序遍历：升序遍历

极端情况，二叉树会变为一根棍子，这样将影响查询效率，因此要让这棵树变的更加平衡

保证性能的关键

1. 保证二维维度！-> 左右子树节点平衡（recursively）
2. Balanced
3. https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree

#### 平衡二叉搜索树

实现方式有很多种：

* 2-3 tree
* AA tree
* AVL tree
* B-tree
* Red-black tree
* Scapegoat tree
* Splay tree
* Treap
* Weight-balanced tree

#### AVL 树

1. 发明者 G.M.Adelson-Velsky 和 Evgenii Landis
2. Balance Factor(平衡因子)：是它的左子树的高度减去它的右子树的高度（有时相反）。balance factor = {-1, 0, 1}
3. 通过旋转操作来进行平衡（四种）
4. https://en.wikipedia.org/wiki/Self-balancing_binary_search_tree

#### 旋转操作

1. 左旋（逆时针，右右子树）
2. 右旋（顺时针，左左子树）
3. 左右旋（左右子树）
4. 右左旋（右左子树）

#### AVL 总结

1. 平衡二叉搜索树
2. 每个节点存balance factor = {-1, 0, 1}
3. 四种旋转操作

不足：节点需要存储额外信息，且调整次数频繁

#### 红黑树

红黑树是一种近似平衡的二叉搜索树（Binary Search Tree），它能够确保任何一个节点的左右子树的高度差小于两倍。具体来说，红黑树是满足如下条件的二叉搜索树：

* 每个节点要么是红色，要么是黑色
* 根节点是黑色
* 每个叶子节点（nil节点、空节点）是黑色的。
* 不能有相邻的两个红色节点
* 从任一节点到其每个叶子节点的所有路径都包含相同数目的黑色节点。

#### 对比

* AVL trees provide faster lookups than Red Black Trees because they are more sticktly balanced.
* Red Black Trees provide faster insertion and removal operations than AVL trees as fewer rotations are done due to relatively relaxed balancing.
* AVL trees store balance factors or heights with each node, thus requires strage for an integer per node whereas Red Black Tree requires only 1 bit of informatioin per node.
* Red Black Trees are used in most of the language libraries like map, multimap, multiset in C++ whereas AVL trees are used in databases where faster retrievals are required.

## Homework

* https://github.com/WiFeng/leetcode/tree/master/climbing-stairs
* https://github.com/WiFeng/leetcode/tree/master/implement-trie-prefix-tree
* https://github.com/WiFeng/leetcode/tree/master/word-ladder

待完成

* https://leetcode-cn.com/problems/friend-circles