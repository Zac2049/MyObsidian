- `unicode.IsDigit`
- `strings.Split(s, " ")`
- `strings.HasPrefix`
- `math.MinInt32`
- `i, err := strconv.Atoi(token)`
- 使用`sort.Search`时，入参条件是根据要查找的序列是升序序列还是降序序列来决定的，如果是升序序列，则传入的条件应该是`>=目标元素值`，如果是降序序列，则传入的条件应该是`<=目标元素值`
	- `func Search(n int, f func(int) bool) int`
	- Search uses binary search to find and return the smallest index i in [0, n) at which f(i) is true, assuming that on the range [0, n), f(i) == true implies f(i+1) == true. 
- 求数字序列的下一个排列
- 单调栈
- [32. 最长有效括号 - 力扣（Leetcode）](https://leetcode.cn/problems/longest-valid-parentheses/description/)
	- dp[i]=dp[i−2]+2
	- dp[i]=dp[i−1]+dp[i−dp[i−1]−2]+2
- 判断重复字符，使用哈希集合这个数据结构（即 `C++` 中的 `std::unordered_set`，`Java` 中的 `HashSet`，`Python` 中的 `set`, `JavaScript` 中的 `Set`，`Golang` 中的 `map[byte]int{}`）
- 使用`strings.Builder`拼接字符串
- [72. 编辑距离 - 力扣（Leetcode）](https://leetcode.cn/problems/edit-distance/description/?favorite=2cktkvj)
	- 最长公共子序列问题
	- ![[Pasted image 20230202103046.png]]
- [146. LRU 缓存 - 力扣（Leetcode）](https://leetcode.cn/problems/lru-cache/description/?favorite=2cktkvj)
	- 双向链表数据结构
- [148. 排序链表 - 力扣（Leetcode）](https://leetcode.cn/problems/sort-list/solutions/492301/pai-xu-lian-biao-by-leetcode-solution/)
	- 为什么不用mid+1
	- 非递归写法
- [152. 乘积最大子数组 - 力扣（Leetcode）](https://leetcode.cn/problems/maximum-product-subarray/solutions/250015/cheng-ji-zui-da-zi-shu-zu-by-leetcode-solution/)
	- ![[Pasted image 20230211214417.png]]
- 自己实现pow，math.pow按照浮点运算
- 闭包的使用一般是申明一个函数变量，再执行之
- [207. 课程表 - 力扣（Leetcode）](https://leetcode.cn/problems/course-schedule/solutions/359392/ke-cheng-biao-by-leetcode-solution/)
	- 拓扑排序
- [208. 实现 Trie (前缀树) - 力扣（Leetcode）](https://leetcode.cn/problems/implement-trie-prefix-tree/solutions/717239/shi-xian-trie-qian-zhui-shu-by-leetcode-ti500/)
	- 前缀树即能存储一组有公共前缀的单词的数据结构
- [300. 最长递增子序列 - 力扣（Leetcode）](https://leetcode.cn/problems/longest-increasing-subsequence/solutions/147667/zui-chang-shang-sheng-zi-xu-lie-by-leetcode-soluti/)
	- dp[i] = max(dp[j]) + 1 if nums[i] > nums[j]
	- 定义 dp[i] 为考虑前 i 个元素，以第 i 个数字结尾的最长上升子序列的长度
- [309. 最佳买卖股票时机含冷冻期 - 力扣（Leetcode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/solutions/323509/zui-jia-mai-mai-gu-piao-shi-ji-han-leng-dong-qi-4/)
	- 股票系列
	- 二维状态（或者多个状态）
	- 贪心
	- 边界初始化
	- 空间优化（或者本身只需记录临界状态）
	- [123. 买卖股票的最佳时机 III - 力扣（Leetcode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/solutions/552695/mai-mai-gu-piao-de-zui-jia-shi-ji-iii-by-wrnt/)
- 记忆化搜索和dp
	- [记忆化搜索 - OI Wiki (oi-wiki.org)](https://oi-wiki.org/dp/memo/)
	- [312. 戳气球 - 力扣（Leetcode）](https://leetcode.cn/problems/burst-balloons/solutions/336390/chuo-qi-qiu-by-leetcode-solution/)
	- 分治+dfs
	- [322. 零钱兑换 - 力扣（Leetcode）](https://leetcode.cn/problems/coin-change/solutions/132979/322-ling-qian-dui-huan-by-leetcode-solution/)
	- 子集问题
- [406. 根据身高重建队列 - 力扣（Leetcode）](https://leetcode.cn/problems/queue-reconstruction-by-height/solutions/486066/gen-ju-shen-gao-zhong-jian-dui-lie-by-leetcode-sol/)
	- 自定义排序题
- [416. 分割等和子集 - 力扣（Leetcode）](https://leetcode.cn/problems/partition-equal-subset-sum/solutions/442320/fen-ge-deng-he-zi-ji-by-leetcode-solution/)
	- 背包问题
	- 和一般的背包不同的是，其dp仅仅表示能否构成（bool），而不是最大值
	- 此题有可视化的图，加强对背包各个状态的理解
- 关于正方形的dp
	- [1277. 统计全为 1 的正方形子矩阵 - 力扣（Leetcode）](https://leetcode.cn/problems/count-square-submatrices-with-all-ones/solutions/101706/tong-ji-quan-wei-1-de-zheng-fang-xing-zi-ju-zhen-2/)
- golang闭包的递归dfs写法
	- [236. 二叉树的最近公共祖先 - 力扣（Leetcode）](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solutions/238552/er-cha-shu-de-zui-jin-gong-gong-zu-xian-by-leetc-2/)
```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    parent := map[int]*TreeNode{}
    visited := map[int]bool{}

    var dfs func(*TreeNode)//事先声明
    dfs = func(r *TreeNode) {//定义
        if r == nil {
            return
        }
        if r.Left != nil {
            parent[r.Left.Val] = r
            dfs(r.Left)
        }
        if r.Right != nil {
            parent[r.Right.Val] = r
            dfs(r.Right)
        }
    }
    dfs(root)

    for p != nil {
        visited[p.Val] = true
        p = parent[p.Val]
    }
    for q != nil {
        if visited[q.Val] {
            return q
        }
        q = parent[q.Val]
    }

    return nil
}
```

- 求最长的字母和数字个数相等的子数组
	- 转化为，字母为1，数字为-1，子数组计数和为0的问题
	- `map[int]int{}` key为前缀和，value为此前缀和第一次出现的下标
	- 维护maxLength和startIndex即可
	- [面试题 17.05. 字母与数字 - 力扣（Leetcode）](https://leetcode.cn/problems/find-longest-subarray-lcci/solutions/2159214/zi-mu-yu-shu-zi-by-leetcode-solution-ezf4/)

- 求除自身以外数组的乘积
	- 定义状态，定义left数组和right数组，`left[i], right[i]`表示i左边（右边）的数字累乘
	- 优化空间，已经计算一边的情况下（比如left数组），计算另一边时直接更新结果，一个数记录累乘值

- 疑问：[239. 滑动窗口最大值 - 力扣（Leetcode）](https://leetcode.cn/problems/sliding-window-maximum/solutions/543426/hua-dong-chuang-kou-zui-da-zhi-by-leetco-ki6m/)
	- 这题的golang的优先队列怎么理解
```go
var a []int
type hp struct{ sort.IntSlice }
func (h hp) Less(i, j int) bool  { return a[h.IntSlice[i]] > a[h.IntSlice[j]] }// hp单纯存着下标，实际按照a[hp的下标]排序
func (h *hp) Push(v interface{}) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() interface{}   { a := h.IntSlice; v := a[len(a)-1]; h.IntSlice = a[:len(a)-1]; return v }
```

- 从回溯的角度去理解01背包和完全背包，及它们的空间优化
	- [0-1背包 完全背包【基础算法精讲 18】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV16Y411v7Y6/?share_source=copy_web&vd_source=a71f9ad96b761fa976590ef0f63f1405)

- 各类双指针中的dp思想
	1. 同向双指针
	- 双指针的核心在于，一个数组中的元素都有相同属性（比如都为正数，都为字符），求最大和（或者最小等，核心可以转化为一个状态）。
	- 先遍历right，`dp[right]`是由right左边的数组决定的，再遍历left，知道满足或者不满足条件

	2. 相向双指针（三数求和，盛最多水的容器，接雨水）
	- 保证有序，`left := 0, right := len(nums)-1`
	- 通过O（1）的时间得到O（n）的信息
	- 指针运动类似快排

	- 接雨水
	- 法一，每个有雨水的元素，称作一个桶，我们求每个元素左边的木板的最大值preMax和右边木板的最大值sufMax（用两个数组以及dp状态的思想维护），和当前元素的底板相比较可得到当前桶的容量。
	- 用双指针的思想优化空间， 核心，
		- `while left <= right`
		- `if preMax < sufMax, contain += preMax - curHeight`
		- `else: contain += sufMax - curHeight`
		- `preMax, sufMax`可用dp思想更新，不用多余空间

- 二分查找
	- 第一个参数n表示要搜索的元素数量，第二个参数f是一个函数，它接受一个元素的索引作为参数，返回一个布尔值表示该元素是否满足要求。如果找到符合要求的元素，`Search`函数会返回该元素的索引；否则，它会返回一个最接近符合要求的元素的索引，该元素可能比目标元素小或大。`index := sort.Search(len(arr), func(i int) bool { return arr[i] >= target })`

```go
// 自己实现的二分查找
func binarySearch(arr []int, target int) int { 
	left, right := 0, len(arr)-1 // 左闭右闭[left, right]
	for left <= right {
		// left + (right - left)/2
		mid := (left + right) / 2
		if arr[mid] == target {
			return mid
		} else if arr[mid] < target {
			left = mid + 1
		} else {
			right = mid - 1
		}
	}
	return -1
}
```

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2  # 防止整数溢出

        if arr[mid] == target:
            return mid  # 找到目标，返回索引
        elif arr[mid] < target:
            left = mid + 1  # 目标在右半部分
        else:
            right = mid - 1  # 目标在左半部分

    return -1  # 目标不在数组中
```
自己实现的二分查找和`sort.Search`函数的主要区别在于实现方式和一些细节问题。以下是一些需要注意的问题：

1.  实现方式：自己实现的二分查找通常需要手动编写查找逻辑和循环控制结构，而`sort.Search`函数是一个高级抽象，可以直接传递一个函数来实现查找逻辑，大大减少了编写代码的工作量。
    
2.  返回值：自己实现的二分查找可以根据实际需要返回不同的值，例如元素的索引、元素本身或者布尔值等。而`sort.Search`函数只能返回元素的索引，这可能不符合所有使用场景的需求。
    
3.  数组类型：`sort.Search`函数只能用于已排序的切片或数组，而自己实现的二分查找可以应用于任何类型的有序数据结构，包括自定义类型和数据库查询结果等。
    
4.  性能：由于`sort.Search`函数是由Go语言的标准库实现的，因此它的性能通常比自己实现的二分查找更高效和可靠。此外，`sort.Search`函数还可以利用多核CPU并行计算，进一步提高性能。
    

在使用自己实现的二分查找时，需要注意以下问题：

1.  循环条件：二分查找的循环条件要保证每次循环后，搜索范围至少缩小了一半。通常可以使用`left <= right`或`left < right`作为循环条件。
    
2.  中间值计算：在计算中间值时，要注意使用整除运算符，避免出现浮点数舍入误差导致的错误。
    
3.  边界条件：当目标元素不存在于数组中时，要特别处理边界情况，例如在循环结束后返回-1或者数组长度等。
    

总之，自己实现的二分查找和`sort.Search`函数都有其优点和缺点，具体使用哪种方式应根据实际需要和代码的复杂度来决定。

- 二分问题，红蓝染色法。
	- 找峰顶（0到n-2中二分，峰顶左侧涂红，峰顶本身和右侧涂蓝，m和m+1比较可知该怎么涂色）二分范围，染色规则
	- 旋转数组的最小值，染色规则类似峰顶，mid和最后一个元素比较，mid < last，min in [0, mid]，else min in (mid, last]

- 链表问题
	- 反转，抛弃以前用的尾插吧
```go
pre = None// 记录pre和nxt，将cur逐渐连至pre，后移pre，cur，nxt
cur = head
while cur!=nil {
	nxt := cur.Next
	cur.next = pre
	pre = cur
	cur = nxt
}
return pre
```


```python
# 建议写成闭包函数，无需返回任何值，两边结点已经反转
def reverseList(head: ListNode):
	pre = None # 
	cur = head
	while cur:
		next = cur.next
		cur.next = pre
		pre = cur
		cur = next
```
k组反转链表注意点：

- 先求链表长度，逐个减k进行反转，不足k跳出循环
- p0是前一组的末指针，p0.next反转后会变至最后，cur反转后到了下一组的头指针


快慢指针
- 求环的入口
	- 环长=b+c
	- 相遇时慢指针移动距离=a+b
	- 相遇时快指针移动距离=a+b+k(b+c)
	- 两倍的关系联立公式，发现规律

两数之和，三数之和
- 因为这个和是唯一解，所以可以用双指针指向的数的和动态调整判断


direction is 2D list`[[0,1],[1,0],[0.-1],[-1.0]]`. `[][0]` presents the row direction, while `[][1]` presents the column

```cpp
// C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组
        auto matrix_new = matrix;
```
```python
# Python 这里不能 matrix_new = matrix 或 matrix_new = matrix[:] 因为是引用拷贝
```

```python
 neighbors = [(1,0), (1,-1), (0,-1), (-1,-1), (-1,0), (-1,1), (0,1), (1,1)]
```

同理，也可以用2D list表示$3\times3$ neighbors

- 使用`mp = collections.defaultdict(list)` 创建带有默认值的dict，这里是默认值为空list，如果括号里为int，默认值为0

- hashable的数据结构不能用做key，但tuple这种可以，所以有时候能把list转成tuple当作key

- python使用math.inf表示最大数，-math.inf最小数

- 最小栈问题，用一个栈保存每个存入状态的最小值，初始为inf，插入取min，取出$O(1)$，空间换时间

- 计算器问题，记录符号的stack，和当前符号变量，遇到-反转，（入栈，）出栈


- 链表右移k个结点。可以连接首尾，head移动n - k % n

- 从前序与中序遍历序列构造二叉树，从中序与后序遍历序列构造二叉树 问题
```
[ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]
[ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]
```
根据这样进行分治，在前序和后序中找到根节点，构建根节点，划分左右区间构建左右子树。
一个注意点
 ```python
# 1.
# 构造哈希映射，帮助我们快速定位根节点
index = {element: i for i, element in enumerate(inorder)}
# 2.
# 得到左子树中的节点数目，方便划分区间
size_left_subtree = inorder_root - inorder_left
```

- 二叉树中的最大路径和问题，用全局变量维护最大路径和`maxSum`，其等于自己和`root.val + leftContribution + rightContribution`，所谓贡献就是非零单向路径和


- Python 中的整数、浮点数和其他数据类型的最小值可以通过一些特定的属性或函数来表示。

1. **整数 (int)**:

   Python 中的整数类型 (`int`) 的最小值是由系统架构决定的。你可以使用 `sys` 模块来获取整数的最小值。示例如下：

   ```python
   import sys
   min_int = sys.maxsize  # 获取系统中整数的最小值
   ```

   `sys.maxsize` 返回当前平台上的最大整数值，通常是 2^31 - 1 或 2^63 - 1，取决于你的操作系统和 Python 版本。

2. **浮点数 (float)**:

   Python 中的浮点数 (`float`) 通常使用特殊常数来表示最小值和最大值。最小的正浮点数可以使用 `sys.float_info` 来获取：

   ```python
   import sys
   min_float = sys.float_info.min  # 获取最小的正浮点数
   ```

   这将返回表示最小正浮点数的值，通常是约 2.2250738585072014e-308。

3. **其他类型**:

   对于其他数据类型，例如字符串、布尔值等，它们没有显式的“最小值”概念，因为它们的取值范围通常由数据类型本身的定义决定。例如，字符串可以为空字符串 (`""`)，布尔值可以是 `True` 或 `False`。

需要注意的是，Python 的整数和浮点数通常没有明确的“最小值”表示负无穷大（negative infinity），因为它们可以取负数。如果需要表示负无穷大，可以使用 `float('-inf')` 来表示。

```python
negative_infinity = float('-inf')  # 表示负无穷大
```

最小值和最大值的确切表示在不同的 Python 版本和系统上可能会有所不同，因此在实际应用中，最好使用 `sys` 和 `sys.float_info` 来获取这些信息，以确保准确性。


- 如何记录树的深度？层次遍历时，保存结点的同时也保存一个深度变量，用dict结合
```python
rightmost_value_at_depth = dict() # 深度为索引，存放节点的值
max_depth = -1

queue = deque([(root, 0)])
while queue:
	node, depth = queue.popleft()

	if node is not None:
		# 维护二叉树的最大深度
		max_depth = max(max_depth, depth)

		# 由于每一层最后一个访问到的节点才是我们要的答案，因此不断更新对应深度的信息即可
		rightmost_value_at_depth[depth] = node.val

		queue.append((node.left, depth + 1))
		queue.append((node.right, depth + 1))

```

```python
# 实际上每次计算size就可以求上一层的宽度
def levelOrder(self, root: TreeNode) -> List[int]:
        if not root:
            return []
        queue = [root]
        order = []
        while queue:
            size = len(queue)
            for _ in range(size):
                curr = queue.pop(0)
                order.append(curr.val)
                if curr.left:
                    queue.append(curr.left)
                if curr.right:
                    queue.append(curr.right)
        return order
```
- 关于python的输入
==注意`sys.stdin.readline().split()`返回的是list
`sys.stdin.readline()`返回的是str==
```python
n,k = sys.stdin.readline().split()
nums = []
for _ in range(n):
    num = map(int, sys.stdin.readline().split())
    nums.append(num)

# or
# for line in sys.stdin.readlines():
for line in sys.stdin:
    line=line.split()
    temp=list(map(int,line))
```

- 克隆图的核心，哈希和dfs
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

from typing import Optional
class Solution:
    def __init__(self):
        self.visited = {}
    def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
        
        if not node:
            return
        
        if node in visited:
            return visited[node]

        clone_node = Node(node.val, [])

        self.visited[node] = clone_node

        if node.neighbors:
            clone_node.neighbors = [self.cloneGraph(n) for n in neighbors]

        return clone_node
```

- python字符转int，"ordinal"（序数）
```python
char = 'A' 
integer_value = ord(char)
```

### 位运算

1. **按位与（&）**：将两个二进制数的对应位进行与操作，只有在两个操作数的对应位都是 1 的时候，结果位才是 1。例如，`1010 & 1100` 将得到 `1000`。

2. **按位或（|）**：将两个二进制数的对应位进行或操作，只要两个操作数的对应位中至少有一个是 1，结果位就是 1。例如，`1010 | 1100` 将得到 `1110`。

3. **按位异或（^）**：将两个二进制数的对应位进行异或操作，只有在两个操作数的对应位不相同的情况下，结果位才是 1。例如，`1010 ^ 1100` 将得到 `0110`。

4. **按位取反（~）**：将一个二进制数的每一位进行取反操作，即 0 变为 1，1 变为 0。例如，`~1010` 将得到 `0101`。

5. **左移（<<）**：将一个二进制数的所有位向左移动指定的位数。例如，`1010 << 2` 将得到 `101000`。

6. **右移（>>）**：将一个二进制数的所有位向右移动指定的位数。右移操作有两种形式，逻辑右移和算术右移。逻辑右移用 0 填充左边的空位，而算术右移用符号位填充左边的空位。


位运算在解决算法和编程问题时非常有用。以下是一些位运算技巧，适用于解决各种算法和编程挑战：

1. **按位与（&）操作**：可以用于提取某一位上的值，清除特定位上的值，或者检查特定位是否为1。

2. **按位或（|）操作**：用于将某一位设置为1，或者将多个位上的值合并到一个整数。

3. **按位异或（^）操作**：用于交换两个变量的值，或者用于在不引入额外变量的情况下交换数组中的元素。

4. **左移（<<）和右移（>>)操作**：用于将一个数左移或右移n位。左移相当于将原数乘以2^n，右移相当于将原数除以2^n。这些操作在优化计算2的幂次方、或者在某些位运算技巧中非常有用。

5. **清除最低位的1**：`n & (n - 1)` 可以用来清除二进制表示中最低位的1。

6. **获取最低位的1**：`n & -n` 可以用来获取最低位的1。

7. **检查奇偶性**：`n & 1` 可以用来检查一个整数是奇数还是偶数。如果结果为1，是奇数；如果结果为0，是偶数。

8. **将整数转为二进制字符串**：`bin(n)` 可以将整数n转换为其二进制表示的字符串。

9. **将字符串转为整数**：`int("1010", 2)` 可以将二进制字符串"1010"转为整数。

10. **检查位是否被设置**：`(n & (1 << i)) != 0` 可以检查第i位是否被设置。

11. **设置位**：`n | (1 << i)` 可以将第i位设置为1。

12. **清除位**：`n & ~(1 << i)` 可以将第i位清零。

13. **更新位**：`(n & ~(1 << i)) | (bit << i)` 可以更新第i位的值为bit。

14. **交换两个数的特定位**：可以使用按位操作和位掩码来交换两个数的特定位。
