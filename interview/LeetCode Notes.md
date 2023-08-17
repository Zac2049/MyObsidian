## 经典150题总结



## 遍历

```go
func preOrder(root *treeNode) []int {
	if root == nil {
		return nil
	}
	result := make([]int, 0)
	stack := make([]*treeNode, 0)
	for root != nil || len(stack) > 0 {
		for root != nil {
			result = append(result, root.val)
			stack = append(stack, root)
			root = root.left
		}
		root = stack[len(stack)-1]
		stack = stack[:len(stack)-1]
		root = root.right
	}
	return result
}

func midOrder(root *treeNode) []int {
	if root == nil {
		return nil
	}
	result := make([]int, 0)
	stack := make([]*treeNode, 0)
	for root != nil || len(stack) > 0 {
		for root != nil {
			stack = append(stack, root)
			root = root.left
		}
		root = stack[len(stack)-1]
		result = append(result, root.val)
		stack = stack[:len(stack)-1]
		root = root.right
	}
	return result
}

func postOrder(root *treeNode) []int {
	if root == nil {
		return nil
	}
	result := make([]int, 0)
	stack := make([]*treeNode, 0)
	var prev *treeNode
	for root != nil || len(stack) != 0 {
		for root != nil {
			stack = append(stack, root)
			root = root.left
		}
		root = stack[len(stack)-1]
		if root.right == nil || root.right == prev {
			result = append(result, root.val)
			stack = stack[:len(stack)-1]
			prev = root
		}
		root = root.right
	}
	return result
}

// dfs
func preOrderTraversal(root *treeNode) []int {
	result := make([]int, 0)
	dfs(root, &result)
	return result
}

func dfs(root *treeNode, result *[]int) {
	if root == nil {
		return
	}
	*result = append(*result, root.val)
	dfs(root.left, result)
	dfs(root.right, result)
}

func postOrderTraversal(root *treeNode) []int {
	result := divideAndConquer(root)
	return result
}

func divideAndConquer(root *treeNode) []int {
	result := make([]int, 0)
	if root == nil {
		return result
	}
	left := divideAndConquer(root.left)
	right := divideAndConquer(root.right)
	result = append(result, root.val)
	result = append(result, left...)
	result = append(result, right...)
	return result
}

func levelOrder(root *treeNode) []int {
	result := make([]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*treeNode, 0)
	result = append(result, root.val)
	queue = append(queue, root)
	for len(queue) != 0 {
		l := len(queue)
		for i := 0; i < l; i++ {
			root = queue[0]
			result = append(result, root.val)
			queue = queue[1:]
			if root.left != nil {
				queue = append(queue, root.left)
			}
			if root.right != nil {
				queue = append(queue, root.right)
			}
		}
	}
	return result
}

// part of sort
// merge sort uses divide & conquer
func mergeSort(nums []int) []int {
	if len(nums) <= 1 {
		return nums
	}
	mid := len(nums) / 2
	left := mergeSort(nums[:mid])
	right := mergeSort(nums[mid:])
	return merge(left, right)
}

func merge(left, right []int) []int {
	i := 0
	j := 0
	result := make([]int, 0)
	for i < len(left) && j < len(right) {
		if left[i] > right[j] {
			result = append(result, right[j])
			j++
		} else {
			result = append(result, left[i])
			i++
		}
	}
	result = append(result, left[i:]...)
	result = append(result, right[j:]...)
	return result
}

func quickSort(nums []int) []int {
	quick(nums, 0, len(nums)-1)
	return nums
}

func quick(nums []int, low, high int) []int {
	if low <= high {
		pivot := partition(nums, low, high)
		quick(nums, low, pivot-1)
		quick(nums, pivot, high)
	}
	return nums
}

func partition(nums []int, low, high int) int {
	p := nums[high]
	for low < high {
		for low < high && nums[low] <= p {
			low++
		}
		nums[high] = nums[low]
		for low < high && p <= nums[high] {
			high--
		}
		nums[low] = nums[high]
	}
	nums[low] = p
	return low
}


func HeapSort(a []int) []int {
    // 1、无序数组a
	// 2、将无序数组a构建为一个大根堆
	for i := len(a)/2 - 1; i >= 0; i-- {
		sink(a, i, len(a))
	}
	// 3、交换a[0]和a[len(a)-1]
	// 4、然后把前面这段数组继续下沉保持堆结构，如此循环即可
	for i := len(a) - 1; i >= 1; i-- {
		// 从后往前填充值
		swap(a, 0, i)
		// 前面的长度也减一
		sink(a, 0, i)
	}
	return a
}
func sink(a []int, i int, length int) {
	for {
		// 左节点索引(从0开始，所以左节点为i*2+1)
		l := i*2 + 1
		// 右节点索引
		r := i*2 + 2
		// idx保存根、左、右三者之间较大值的索引
		idx := i
		// 存在左节点，左节点值较大，则取左节点
		if l < length && a[l] > a[idx] {
			idx = l
		}
		// 存在右节点，且值较大，取右节点
		if r < length && a[r] > a[idx] {
			idx = r
		}
		// 如果根节点较大，则不用下沉
		if idx == i {
			break
		}
		// 如果根节点较小，则交换值，并继续下沉
		swap(a, i, idx)
		// 继续下沉idx节点
		i = idx
	}
}
func swap(a []int, i, j int) {
	a[i], a[j] = a[j], a[i]
}

```

## 背包问题

```go
//每个物品可取无限次
//f[i][j]为只能选前i个物品时，容量为j的背包可以达到的最大价值。
func packComplete() {
	W := 10
	w := [5]int{3, 2, 2, 3, 3}
	v := [5]int{1, 1, 2, 2, 3}
	n := 4
	f := [100]int{}
	for i := 0; i <= n; i++ {
		for l := w[i]; l <= W; l++ {
			f[l] = max(f[l], f[l-w[i]]+v[i]) // l>w[i]时背包剩余容量大于单个物品重量，f[i][l]被f[i][l-w[i]]所影响，l从小到大考虑放入多个第i个商品
			fmt.Printf("f[%d]: %d  ", l, f[l])
		}
		// f[i][l+w[i]] = max(f[i-1][l+w[i]], f[i-1][l]+w[i])
		// f[i][l+w[i]] = max(max(f[i-1][l+w[i]], f[i-1][l]+w[i]), f[i][l+w[i]])
	}
	fmt.Println("")
}

//一个物品仅取一次
func pack01() {
	W := 10
	w := [5]int{0, 2, 2, 3, 3}
	v := [5]int{0, 1, 2, 2, 3}
	n := 4
	f := [100]int{}
	for i := 1; i <= n; i++ {
		for l := W; l >= w[i]; l-- {
			f[l] = max(f[l], f[l-w[i]]+v[i]) // l代表背包剩余容量，l从大到小从上个i-1中得到
			fmt.Printf("f[%d]: %d   ", l, f[l])
		}
	}
}

var W = 10
var V = W

func zeroOnePark(f []int, c, w int) {
	for v := V; v >= c; v++ {
		f[v] = max(f[v], f[v-c]+w)
	}
}

func completePack(f []int, c, w int) {
	for v := c; v >= V; v++ {
		f[v] = max(f[v], f[v-c]+w)
	}
}

func multiplePack(f, c, w, m []int, i int) {
	if c[i]*m[i] >= V {
		completePack(f, c[i], w[i])
	}
	k := 1
	for k < m[i] {
		zeroOnePark(f, k*c[i], k*w[i])
		m[i] -= k
		k <<= 1
	}
	zeroOnePark(f, m[i]*c[i], m[i]*w[i])
}

func max(a, b int) int {
	if a > b {
		return a
	} else {
		return b
	}
}
```

## 滑动窗口

[30. 串联所有单词的子串 - 力扣（Leetcode）](https://leetcode.cn/problems/substring-with-concatenation-of-all-words/solutions/1616997/chuan-lian-suo-you-dan-ci-de-zi-chuan-by-244a/)

```go
func findAnagrams(s string, p string) []int {
	ori := map[byte]int{}
	for i := 0; i < len(p); i++ {
		ori[p[i]]++
	}
	cnt := map[byte]int{}
	ans := []int{}
	match := 0
	for l, r := 0, 0; r < len(s); {
		if ori[s[r]] != 0 {
			cnt[s[r]]++
			if ori[s[r]] == cnt[s[r]] {
				match++
			}
		}
		r++
		// 当窗口长度大于等于字符串长度，缩紧窗口
		for r-l >= len(p) {// 大于等于和等于无差别
		// 当窗口长度和字符串匹配，并且里面每个字符数量也匹配时，满足条件
			if r-l == len(p) && match == len(ori) {
				ans = append(ans, l)
			}
			if ori[s[l]] != 0 {
				if cnt[s[l]] == ori[s[l]] {
					match--
				}
				cnt[s[l]]--
			}
			l++
		}
	}
	return ans
}



func findAnagrams(s, p string) (ans []int) {
    sLen, pLen := len(s), len(p)
    if sLen < pLen {
        return
    }

    count := [26]int{}
    for i, ch := range p {
        count[s[i]-'a']++
        count[ch-'a']--
    }

    differ := 0
    for _, c := range count {
        if c != 0 {
            differ++
        }
    }
    if differ == 0 {
        ans = append(ans, 0)
    }
	
    for i, ch := range s[:sLen-pLen] {
        if count[ch-'a'] == 1 { // 窗口中字母 s[i] 的数量与字符串 p 中的数量从不同变得相同
                differ--
        } else if count[ch-'a'] == 0 { // 窗口中字母 s[i] 的数量与字符串 p 中的数量从相同变得不同
            differ++
        }
        count[ch-'a']--

        if count[s[i+pLen]-'a'] == -1 { // 窗口中字母 s[i+pLen] 的数量与字符串 p 中的数量从不同变得相同
            differ--
        } else if count[s[i+pLen]-'a'] == 0 { // 窗口中字母 s[i+pLen] 的数量与字符串 p 中的数量从相同变得不同
            differ++
        }
        count[s[i+pLen]-'a']++

        if differ == 0 {
            ans = append(ans, i+1)
        }
    }
    return
}
// 优化代码，令differ := map[int]int, 用哈希表统计单个字母的不同数
```

## 优先队列

[Go标准库中文文档 (cngolib.com)](http://cngolib.com/container-heap.html)
<iframe 
		border=0
		frameborder=0
		height=550
		width=850
		src="http://cngolib.com/container-heap.html"></iframe>

[1801. 积压订单中的订单总数 - 力扣（Leetcode）](https://leetcode.cn/problems/number-of-orders-in-the-backlog/description