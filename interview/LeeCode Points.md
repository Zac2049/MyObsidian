### Use Golang
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


### Python points

#### 按元素排序
```python
intervals.sort(key=lambda x: x[0])
intervals.sort(key=itemgetter(0))

key=lambda student: student.age
key=attrgetter('age')

key=itemgetter(1,2)# 索引 属性
key=attrgetter('grade', 'age')
```
### 链表反转
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
`sys.stdin.readline()`返回的是str，一般会多一个回车符==
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

实现二进制字符串按位加法：
```python
class Solution:
    def addBinary(self, a, b) -> str:
        x, y = int(a, 2), int(b, 2)
        while y:
            answer = x ^ y
            carry = (x & y) << 1
            x, y = answer, carry
        return bin(x)[2:]
```


位运算在解决算法和编程问题时非常有用。以下是一些位运算技巧，适用于解决各种算法和编程挑战：

1. **按位与（&）操作**：可以用于==提取==某一位上的值，==清除==特定位上的值，或者检查特定位是否为1。
	& 不一样的位 表示==清除==
1. **按位或（|）操作**：用于将某一位设置为1，或者将多个位上的值合并到一个整数。
	| 1 表示==设置1==
1. **按位异或（^）操作**：用于交换两个变量的值，或者用于在不引入额外变量的情况下交换数组中的元素。
	^ 表示==位加==
1. **左移（<<）和右移（>>)操作**：用于将一个数左移或右移n位。左移相当于将原数乘以2^n，右移相当于将原数除以2^n。这些操作在优化计算2的幂次方、或者在某些位运算技巧中非常有用。

2. **清除最低位的1**：`n & (n - 1)` 可以用来清除二进制表示中最低位的1。

3. **获取最低位的1**：`n & -n` 可以用来获取最低位的1。

4. **检查奇偶性**：`n & 1` 可以用来检查一个整数是奇数还是偶数。如果结果为1，是奇数；如果结果为0，是偶数。

5. **将整数转为二进制字符串**：`bin(n)` 可以将整数n转换为其二进制表示的字符串。

6. **将字符串转为整数**：`int("1010", 2)` 可以将二进制字符串"1010"转为整数。

7. **检查位是否被设置**：`(n & (1 << i)) != 0` 可以检查第i位是否被设置。

8. **设置位**：`n | (1 << i)` 可以将第i位设置为1。

9. **清除位**：`n & ~(1 << i)` 可以将第i位清零。

10. **更新位**：`(n & ~(1 << i)) | (bit << i)` 可以更新第i位的值为bit。

11. **交换两个数的特定位**：可以使用按位操作和位掩码来交换两个数的特定位。

#### 搜索旋转数组最小值：
```python
class Solution:
    def findMin(self, nums: List[int]) -> int:
        n = len(nums)
        left, right = 0, n-1

        while left < right :
            mid = (left + right)//2
            if nums[mid] < nums[right] :
                right = mid
            else:
                left = mid + 1    
# 最后输出已经暗示left为目标指针，right为开区间

        return nums[left]
```

类似：
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums:
            return -1
        l, r = 0, len(nums) - 1
        while l <= r:
            mid = (l + r) // 2
            if nums[mid] == target:
                return mid
            if nums[0] <= nums[mid]:
                if nums[0] <= target < nums[mid]:
                    r = mid - 1
                else:
                    l = mid + 1
            else:
                if nums[mid] < target <= nums[len(nums) - 1]:
                    l = mid + 1
                else:
                    r = mid - 1
        return -1
```

#### 银行家算法 贪心+堆排序
```python
class Solution:
    def findMaximizedCapital(self, k: int, w: int, profits: List[int], capital: List[int]) -> int:
    # k次根据，手中的资产无偿得到利益，w初始资产
        if w > max(capital) :
            return w + sum(nlargest(k, profits))

        n = len(profits)
        cur = 0
        arr = [(capital[i], profits[i]) for i in range(n) ]
        arr.sort(key = lambda x: x[0])

        pq = []

        for _ in range(k):
            while cur < n and arr[cur][0] <= w:
                heappush(pq, -arr[cur][1]) # heapq默认完成小根堆，用-可以表示大根堆的关系
                cur += 1
            
            if pq:
                w -= heappop(pq)
            else:
                break
        
        return w

```

### DP相关
##### 最大和的连续子数组
以下是一个Python实现：

```python
def max_subarray_sum(nums):
    if not nums:
        return 0

    current_sum = max_sum = nums[0]

    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)

    return max_sum

# 例子
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_sum(arr)
print(result)
```

这个函数 `max_subarray_sum` 接受一个整数数组 `nums`，返回数组中最大和的连续子数组的和。算法使用了动态规划的思想，通过维护当前和 `current_sum` 和最大和 `max_sum` 来逐步更新结果。遍历数组，对于每个元素，选择要么加入当前子数组，要么重新开始一个新的子数组，取两者中的较大值。最后，`max_sum` 就是所需的最大子数组和。

==动态规划，注意题目要求解，有的递推到最后状态得到解，有的解在中间状态，比如dp的最大值不一定在结尾，可能在中间==

#### 多维DP
##### 三角形最小路径和问题
遍历顺序，每行应该从右向左遍历，才能用一维数组优化空间，因为当前i是由i和i-1决定，i最右边有边界

##### 障碍路径问题 
每行遇到障碍需要置零并跳到下一行（break）


### 单调栈

#### 上一个（下一个）更大（小）元素
给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        ans = [0] * n
        st = []
        for i in range(n-1, -1, -1):
	        t = temperatures[i]
            while st and t >= temperatures[st[-1]]:
                st.pop()
            if st:# 栈顶就是下一个最大
	            ans[i] = st[-1] - i
            st.append(i)
        return ans
        # 从左到右
        for i, t in temperatures:
	        while st and t > temperatures[st[-1]] :# 因为外循环默认保证栈有上个迭代的元素，比较即可
		        j = st.pop()# 栈顶，j是以前判断过的
		        ans[j] = i-j# 当前（循环条件）大于栈顶 实时更新
		    st.append(i)
		# 单调栈的核心是栈顶代表下个更大或者下个更小的数
		# 从右到左 把下一个更大的数存在栈 先pop掉，再更新ans
		# 从左向右 把没有找到的那些数存入 ans更新同pop一起
		# 这里的栈都是从大到小，因为求最大的元素
```

这类题目都有类似在直方图上做一些规则计算。
栈（队列deque）记录的是下标index，`temporatures[index]`是单调的值，外循环`stack.append`，内循环`stack.pop`维持栈的单调性，并更新答案。

> 我建议用状态机画一下

> 接雨水

> 滑动窗口的最大值

```python
from typing import List

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        ans = []
        st = []  # 用于存储当前窗口中元素的索引
		# 也可以用deque pop popleft 
        for i in range(n):
            # 移除窗口外的元素
            while st and st[0] < i - k + 1:
                st.pop(0) # popleft

            # 保持窗口单调递减
            while st and nums[i] > nums[st[-1]]:
                st.pop()

            st.append(i)

            # 记录窗口最大值
            if i >= k - 1:
                ans.append(nums[st[0]])

        return ans
```


### 盛最多水的容器
本质是双指针，实际是状态转移，可证明，这个状态转移能满足目标函数最大

### 滑动窗口
无重复字符的最长子串
map或者dict都行，set则用remove，add方法，in判断存在。==右指针right==是个==独立变量==
```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        n = len(s)
        left, right = 0, 0
        map = DefaultDict(int)

        if not s :
            return 0
        ans = 1

        for left in range(n):
            while right < n : # 并非初始化为left
                if map[s[right]] > 0 :
                    break
                map[s[right]] += 1
                ans = max(ans, right - left+1)
                right += 1
            map[s[left]] -= 1
        return ans
        # set
        # 哈希集合，记录每个字符是否出现过
        occ = set()
        n = len(s)
        # 右指针，初始值为 -1，相当于我们在字符串的左边界的左侧，还没有开始移动
        rk, ans = -1, 0
        for i in range(n):
            if i != 0:
                # 左指针向右移动一格，移除一个字符
                occ.remove(s[i - 1])
            while rk + 1 < n and s[rk + 1] not in occ:
                # 不断地移动右指针
                occ.add(s[rk + 1])
                rk += 1
            # 第 i 到 rk 个字符是一个极长的无重复字符子串
            ans = max(ans, rk - i + 1)
        return ans

```

### 和为K的子数组
如何用前缀和加上哈希表，遍历一次数组，记录之前的信息，`pre_after - pre_before = sum of sub_array`

```python
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        n = len(nums)
        ans = 0
        map = DefaultDict(int)
        map[0] = 1 # 初始条件
        pre_sum = 0
        for num in nums :
            pre_sum += num
            if map[pre_sum - k] > 0 :
                ans += map[pre_sum-k] 
            map[pre_sum] += 1

        return ans
```


### 图&回溯

#### 全排列
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        def backtrack(idx):
            if idx == n:
                ans.append(nums[:])# append 副本
            for i in range(idx, n):
                nums[idx], nums[i] = nums[i], nums[idx]
                backtrack(idx+1)
                nums[idx], nums[i] = nums[i], nums[idx]
        
        n = len(nums)
        ans = []
        backtrack(0)
        return ans
    # 非字典序
```


#### N皇后

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        solutions = []
        board = [-1]*n # row to col
        def track(cur_r):
            if cur_r == n:
                solutions.append(['.'*i + 'Q' + '.'*(n-i-1) for i in board])
            for col in range(n):
                if is_safe(cur_r, col) :
                    board[cur_r] = col
                    track(cur_r+1)
			        # 为什么board没有pop的操作？
			        # 还是需要的
			        board[cur_r] = -1
        def is_safe(cur_r, col):
            for i in range(cur_r):
                in_col = board[i]
                if in_col == col or i+in_col == cur_r + col or i - in_col == cur_r - col:
                    return False
            return True
        
	    track(0)
        return solutions
```

```scheme
(define (queens board-size)
  (define (queen-cols k)
    (if (= k 0)
        (list ())
        (filter
         (lambda (positions) 
           (safe? k positions))
         (flatmap
          (lambda (rest-of-queens)
            (map (lambda (new-row)
                   (adjoin-position new-row k rest-of-queens))
                 (enumerate-interval 1 board-size)))
          (queen-cols (- k 1))))))
```

#### 3维地牢路径长问题poj 2251
求最短路问题，直接**BFS**

```python
from collections import deque

class SE:
    def __init__(self, l, r, c, depth):
        self.l = l
        self.r = r
        self.c = c
        self.depth = depth

def BFS(k, i, j):
    global shortminute
    visited = [[[False for _ in range(40)] for _ in range(40)] for _ in range(40)]
    queue = deque([SE(k, i, j, 1)])
    
    while queue:
        x = queue.popleft()
        if x.l == e.l and x.r == e.r and x.c == e.c:
            shortminute = x.depth
            return True
        
        directions = [(0, 0, -1), (0, -1, 0), (0, 0, 1), (0, 1, 0), (-1, 0, 0), (1, 0, 0)]
        for dx, dy, dz in directions:
            to_l, to_r, to_c = x.l + dx, x.r + dy, x.c + dz
            if 0 <= to_l < L and 0 <= to_r < R and 0 <= to_c < C and maze[to_l][to_r][to_c] and not visited[to_l][to_r][to_c]:
                visited[to_l][to_r][to_c] = True
                queue.append(SE(to_l, to_r, to_c, x.depth + 1))
    
    return False

if __name__ == "__main__":
    shortminute = 0
    while True:
        L, R, C = map(int, input().split())
        if L == 0 and R == 0 and C == 0:
            break
        
        # Initial
        maze = [[[False for _ in range(40)] for _ in range(40)] for _ in range(40)]
        
        # Structure the Maze
        for k in range(1, L + 1):
            for i in range(1, R + 1):
                row = input()
                for j, temp in enumerate(row, start=1):
                    if temp == '.':
                        maze[k][i][j] = True
                    if temp == 'S':
                        maze[k][i][j] = True
                        s = SE(k, i, j, 0)
                    if temp == 'E':
                        maze[k][i][j] = True
                        e = SE(k, i, j, 0)
        
        # Search the min Minute
        if BFS(s.l, s.r, s.c):
            print(f"Escaped in {shortminute-1} minute(s).")
        else:
            print("Trapped!")

```

#### 烂橘子问题
多源BFS，DFS会涉及并行的问题

#### 课程的问题，拓扑排序
1. 图的结构，用字典树，python中`defaultdict(list)`
2. 三个状态的dfs，未访问，已访问，回溯
```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        edges = collections.defaultdict(list)
        visited = [0]*numCourses
        for info in prerequisites:
            edges[info[1]].append(info[0])
        valid = True
        result = []

        def dfs(u: int):
            nonlocal valid
            visited[u] = 1

            for v in edges[u]:
                if visited[v] == 0 :
                    dfs(v)
                    if not valid:
                        return
                elif visited[v] == 1:
                    valid = False
                    return
            visited[u] = 2 # 已完成，回溯，相当于回收
            result.append(u)
        
        for i in range(numCourses):
            if valid and not visited[i]:
                dfs(i)

        return valid
```

#### 组合总和（用栈的append和pop模仿参数传递）
有些candidates的数字，有放回的组合成target
```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        def traceback(idx, target):
            if not target:
                ans.append(cand[:])
                return
            if idx == n:
                return
            traceback(idx+1, target)
            if target - candidates[idx] >= 0:
                cand.append(candidates[idx])
                traceback(idx, target - candidates[idx])  
                cand.pop()# 实际上cand的append和pop是一种对函数参数的模仿
                # 这也就是回溯实际的意义
        ans = list()
        cand = list()
        n = len(candidates)
        traceback(0, target)
        return ans
```


#### 括号生成
递归函数左右括号数**两**个参数，这样才能判断能不能进行右括号的递归操作

#### 分割回文串
```python
class Solution:
    def partition(self, s: str) -> List[List[str]]:
        
        n = len(s)
        f = [[True]*n for _ in range(n)]
		# 预处理得到每个s[i:j]是否回文，为什么按照这个遍历是对的？
		# 因为f[i][j]由f[i+1][j-1]转移，即按照这个顺序
        for i in range(n-1, -1, -1):
            for j in range(i+1, n):
                f[i][j] = (s[i] == s[j]) and f[i+1][j-1]

        ans = []
        cur = []
        # 回溯分割，idx, i 
        def track(idx):
            if idx == n:
                ans.append(cur[:])
                return
            for i in range(idx, n):
                if f[idx][i]:
                    cur.append(s[idx:i+1])
                    track(i+1)
                    cur.pop()
        track(0)
        return ans

```

## 字典问题
`map = defaultdict(Node)`
`if head not in map:` 而不是 `if not map[head]`，后者会报错

### Trie树
```python
class TrieNode:
    def __init__(self):
        self.children = {}  # 用于存储子节点
        self.is_end_of_word = False  # 表示是否为单词的结尾

class Trie:
    def __init__(self):
        self.root = TrieNode()  # 根节点

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()# char--node(char, node)
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        node = self._get_last_node(word)
        return node is not None and node.is_end_of_word

    def startsWith(self, prefix):
        return self._get_last_node(prefix) is not None

    def _get_last_node(self, prefix):
        node = self.root
        for char in prefix:
            if char not in node.children:
                return None
            node = node.children[char]
        return node

# 示例用法：
trie = Trie()
trie.insert("apple")
print(trie.search("apple"))  # 输出 True
print(trie.search("app"))    # 输出 False
print(trie.startsWith("app"))  # 输出 True
trie.insert("app")
print(trie.search("app"))    # 输出 True

```
## 合并K个有序链表
如何按照attr进行堆的维护

```python
ListNode.__lt__ = lambda a, b: a.val < b.val  # 让堆可以比较节点大小

class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        cur = dummy = ListNode()  # 哨兵节点，作为合并后链表头节点的前一个节点
        h = [head for head in lists if head]  # 初始把所有链表的头节点入堆
        heapify(h)  # 堆化
        while h:  # 循环直到堆为空
            node = heappop(h)  # 剩余节点中的最小节点
            if node.next:  # 下一个节点不为空
                heappush(h, node.next)  # 下一个节点有可能是最小节点，入堆
            cur.next = node  # 合并到新链表中
            cur = cur.next  # 准备合并下一个节点
        return dummy.next  # 哨兵节点的下一个节点就是新链表的头节点

```




## 二叉树问题
### 二叉树对称问题
并非单参数的函数递归，而是双参数（左右分别游走）的函数递归

### 判断二叉树是否BST
三参数递归，root，lower val(init as -inf)，upper val(init ans inf)

- 思考这么一个问题，到底如何确定要递归函数的==各个参数==，是否像SICP中说的不动点问题

### 路径总和三
类似于归并或者快速的二重递归，第一层按原target，第二层两参数的递归下降
```python
class Solution:
    def pathSum(self, root: TreeNode, targetSum: int) -> int:
        def rootSum(root, targetSum):
            if root is None:
                return 0

            ret = 0
            if root.val == targetSum:
                ret += 1

            ret += rootSum(root.left, targetSum - root.val)
            ret += rootSum(root.right, targetSum - root.val)
            return ret
        
        if root is None:
            return 0
            
        ret = rootSum(root, targetSum)
        ret += self.pathSum(root.left, targetSum)
        ret += self.pathSum(root.right, targetSum)
        return ret
```

### lowest Common Ancestor问题
状态函数怎么定义
```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        ans = None
        def find(root):
            nonlocal ans
            if not root:
                return None
            cur = False
            if root == p or root == q:
                cur = True
            l = find(root.left)
            r = find(root.right)
            if not l and not r:
                return cur
            if l and r or ( cur and l ) or (cur and r) :
                ans = root
                return False
            if l or r :
                return True
            return False
        find(root)
        return ans
```

### 最大路径和问题
cur 是当前节点的路径和，是内部状态，和贡献值（返回）要分离

```python
class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        # 初始化 ans 为负无穷，确保能够正确比较路径和
        ans = float('-inf') 

        def recur(root):
            nonlocal ans
            if not root:
                return 0

            # 递归计算左右子树的贡献值
            left_contri = max(0, recur(root.left))
            right_contri = max(0, recur(root.right))

            # 更新当前节点的路径和
            cur = left_contri + right_contri + root.val

            # 更新全局最大路径和
            ans = max(ans, cur)

            # 返回当前节点的贡献值（用于上层节点的计算）
            return max(left_contri, right_contri) + root.val

        recur(root)
        return ans

```

