- **is** 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等。a is b 相当于 id(a) == id(b)，id()能够获取对象的内存地址。
- GIL
	为防止竞争条件
	Python 使用引用计数来进行内存管理（垃圾回收）。引用计数就是说 Python 里创建的所有对象，Python 都有一个变量（reference count）记录着当前有多少个引用指向了这个对象，当引用数变成 0 的时候，Python 就会回收这个对象所占用的内存。
	
	当有多个线程同时改一个对象的引用计数的时候，有可能导致内存泄漏（对象的引用计数永远没有归零的机会），还有可能导致对象提起释放，触发莫名奇妙的 bug，程序崩溃（一个对象存在引用的情况下引用计数变成了 0，导致此对象提前释放）。
	
	GIL 是一个锁，或者一把锁（这里强调单个），这把锁加载了 Python 的解释器上，它要求任何 Python 代码在执行的时候需要先申请这把锁，否则就别想执行。GIL 并不是它语言本身的特性，而是 CPython 解释器的实现特性。Python 代码被编译后的字节码会在解释器中执行，在执行过程中，存在于 CPython 解释器中的 GIL 会致使在同一时刻只有一个线程可以执行字节码。
	在提到开发性能瓶颈的时候，我们经常把对资源的限制分为两类，一类是计算密集型（CPU-bound），一类是 I/O 密集型（I/O-bound）。
	
	GIL 提供了线程锁，导致在任意时刻只有一个线程在运行。对于一个完全是计算密集型的程序来说，GIL 锁就会严重的影响性能了。在同一时刻，即使在多核 CPU 上，也仅有一个线程在获得该全局锁后才可以执行字节码，其他的线程想要执行字节码就需要等待该全局锁被释放，所以未能实现真正的并行执行，而是一种**多线程交替执行的串行执行（伪多线程，无法利用多核资源）**。因为在多个线程执行过程中也涉及到了全局锁的获取和释放，上下文环境的切换等。并且相较于单核 CPU，这种效率降低的情况在多核 CPU 上在可能会更加显著。python3.2版本后，GIL锁以固定的时间间隔来进行线程的切换，在其他线程请求获取 GIL 时，当前运行的线程会以 5 毫秒（默认时间）为间隔尝试释放 GIL，另外，GIL 会在遇到 IO操作时被释放并交由其他线程继续执行，比如网络 IO 操作、文件读写等
	
	虽然GIL可以确保在任何时候只有一个线程可以执行Python字节码，但它并不能保证Python多线程的线程安全。GIL 保证的是每一条字节码在执行过程中的独占性，即每一条字节码的执行都是原子性的。GIL 具有释放机制，所以 GIL 并不会保证字节码在执行过程中线程不会进行切换，即在多个字节码之间，线程具有切换的可能性。为了确保线程安全，程序员需要使用同步机制，例如锁、信号量和条件变量，以确保在访问共享资源时只有一个线程可以访问它。

-   使用多进程来代替多线程。用multiprocessing替代Thread
-   将计算密集型任务转移到 Python 的 C / C++ 扩展模块中完成。


- 多线程
	通过threading模块提供线程的功能

- 细粒度
	细粒度是一个相对的概念，通常用于描述某个系统、任务或操作的精细程度或粒度大小。在计算机科学中，细粒度通常指的是操作或任务的粒度较小，即操作或任务被分解为更小的组件或子任务，以便更好地控制和管理。
	
	例如，在并行计算中，细粒度任务是指可以被分解为更小的任务并在多个处理器上并行执行的任务。这些任务通常需要更多的通信和同步开销，但可以更好地利用计算资源。
	
	在机器学习中，细粒度特征是指更小的特征或属性，例如单词或短语，而不是更大的特征或属性，例如句子或段落。这些细粒度特征可以提高模型的准确性和泛化能力。
	
	在自然语言处理中，细粒度分类是指将文本分类为更具体的类别，例如将电影评论分类为正面、负面或中性，而不是将它们分类为更一般的类别，例如电影或书籍。
	
	总之，细粒度是一个相对的概念，通常用于描述操作、任务或系统的精细程度或粒度大小。在计算机科学中，细粒度通常指的是操作或任务的粒度较小，以便更好地控制和管理。

- 多数元素
```python
class Solution: 
	def majorityElement(self, nums: List[int]) -> int: 
		counts = collections.Counter(nums) 
		return max(counts.keys(), key=counts.get) 
```
collections 模块中的 Counter 类来创建一个字典 counts，其中键是 nums 中出现的元素，值是该元素在 nums 中出现的次数。
max() 函数的第一个参数是 counts.keys()，它返回字典 counts 中的所有键。max() 函数的第二个参数是 key=counts.get，它告诉 max() 函数使用字典 counts 的 get() 方法来计算每个键对应的值，并返回值最大的键。

- sorted() 函数的用法与 sort() 函数类似，但它不会修改原列表，而是返回新的排序后的列表。

- 生成器表达式是一种 Python 中的语法结构，用于生成一个迭代器，可以用于循环、列表、元组、集合等数据类型的构建和处理。下面是生成器表达式的一些语法说明：

1. 生成器表达式的基本语法为：`(expression for item in iterable)`，其中 expression 表示要生成的元素，item 表示 iterable 中的每个元素，可以是列表、元组、集合等可迭代对象。

2. 可以在生成器表达式中使用多个迭代器，例如：`(expression for item1 in iterable1 if condition1 for item2 in iterable2 if condition2)`。

3. 可以在生成器表达式中使用多个表达式，例如：`((expression1, expression2) for item in iterable)`。

4. 生成器表达式可以与其他 Python 的语法结构一起使用，例如：`(expression for item in iterable if condition) for outer_item in outer_iterable`。

5. 生成器表达式生成的是一个迭代器，可以使用 next() 函数或 for 循环进行遍历。

6. 生成器表达式可以用于构建列表、元组、集合等数据类型，例如：`[expression for item in iterable if condition]`，其中方括号表示列表，大括号表示集合，圆括号表示元组。

需要注意的是，生成器表达式和列表推导式（list comprehension）的语法结构类似，但是生成器表达式生成的是一个迭代器，而列表推导式生成的是一个列表。在处理大量数据时，生成器表达式比列表推导式更加高效，因为它不会一次性生成所有的数据，而是按需生成，减少了内存消耗。

对满足条件的数进行求和
```python
sum(1 for i in range(lo, hi + 1) if nums[i] == left)
```

- `choice()` 是 Python 中 `random` 模块提供的一个函数，用于在序列中随机选择一个元素。

- python 逆向遍历，从m-1到0
```python
for i in range(m - 1, -1, -1):
```

- 创建一/二维列表，个人猜测，$*$应该是重载了for
```python
lst = [0] * n
# 创建一个包含n个数字的列表
lst = [i for i in range(1, n+1)]

matrix = [[0] * cols for _ in range(rows)]
# 将矩阵中的每个元素设置为其行数和列数的和
matrix = [[i + j for j in range(cols)] for i in range(rows)]
```

- 关于二分查找我的误区
```python
# 这里应该有=符号，必须都遍历为止，包括还有两个和一个元素的情况
while low <= high:
	mid = (low + high) // 2
	if numbers[mid] == target - numbers[i]:
		return [i + 1, mid + 1]
	elif numbers[mid] > target - numbers[i]:
		high = mid - 1 # 需要-1，因为这里的low和high是界限而不是区间
	else:
		low = mid + 1
```

创建字典：
```python
# 字典推导式
apping = {str(i+1): chr(i+97) for i in range(26)}

# 循环
for i in range(1, 27):
    mapping[str(i)] = chr(i + 96)
```
- 之前的误区，只有int:int的map需要判断存不存在。Python的`for key in some map`语句会默认在字典的键中查找是否存在键的项，而不是在值中查找

在 Python 中，推导式（Comprehension）是一种简洁而强大的语法，用于从一个序列（列表、元组、集合、字典等）中快速生成另一个序列。Python中支持以下四种类型的推导式：

1. 列表推导式（List Comprehension）：用于从一个列表中生成另一个列表。
    
    语法：`[expression for item in iterable if condition]`
    
    示例：`new_list = [x**2 for x in range(1, 11) if x % 2 == 0]`
    
2. 字典推导式（Dictionary Comprehension）：用于从一个字典中生成另一个字典。
    
    语法：`{key_expression: value_expression for expression in iterable if condition}`
    
    示例：`new_dict = {i: i**2 for i in range(1, 11)}`
    
3. 集合推导式（Set Comprehension）：用于从一个可迭代对象中生成一个集合。
    
    语法：`{expression for expression in iterable if condition}`
    
    示例：`new_set = {x**2 for x in range(1, 11) if x % 2 == 0}`
    
4. 生成器推导式（Generator Comprehension）：用于从一个可迭代对象中生成一个生成器。
    
    语法：`(expression for expression in iterable if condition)`
    
    示例：`new_generator = (x**2 for x in range(1, 11) if x % 2 == 0)`
    

以上四种推导式都可以使用 if 条件语句进行过滤，语法类似，只是表达式的部分不同。使用推导式可以简化代码，提高代码的可读性和可维护性。

- python的静态图和动态图
---
在 Python 中，可以使用 `del` 关键字或 `pop()` 方法来移除字典中的元素。

1. 使用 del 关键字

`del` 关键字可以用于删除字典中的指定键值对。例如，要删除字典 `dict1` 中的键为 `'a'` 的键值对，可以使用以下代码：

```python
dict1 = {'a': 1, 'b': 2, 'c': 3}
del dict1['a']
print(dict1)   # 输出 {'b': 2, 'c': 3}
```

2. 使用 pop() 方法

`pop()` 方法可以用于删除字典中指定键的键值对，并返回该键对应的值。如果指定的键不存在，则可以返回一个默认值或者抛出一个异常。例如，要删除字典 `dict1` 中的键为 `'a'` 的键值对，并返回该键对应的值，可以使用以下代码：

```python
dict1 = {'a': 1, 'b': 2, 'c': 3}
value = dict1.pop('a')
print(dict1)   # 输出 {'b': 2, 'c': 3}
print(value)   # 输出 1
```

如果指定的键不存在，则可以返回一个默认值或者抛出一个异常。例如，要删除字典 `dict1` 中的键为 `'d'` 的键值对，并返回默认值 `0`，可以使用以下代码：

```python
dict1 = {'a': 1, 'b': 2, 'c': 3}
value = dict1.pop('d', 0)
print(dict1)   # 输出 {'a': 1, 'b': 2, 'c': 3}
print(value)   # 输出 0
```

需要注意的是，使用 `del` 关键字和 `pop()` 方法都会修改原始字典。如果希望在不修改原始字典的情况下删除指定键值对，可以使用字典解析式或 `filter()` 函数。例如，要删除字典 `dict1` 中键为 `'a'` 的键值对，可以使用以下代码：

```python
dict1 = {'a': 1, 'b': 2, 'c': 3}
dict1 = {k: v for k, v in dict1.items() if k != 'a'}
print(dict1)   # 输出 {'b': 2, 'c': 3}
```

在这个例子中，我们使用字典解析式来创建一个新的字典，其中不包含键为 `'a'` 的键值对。

---
在 Python 中，可以使用以下几种方法向字典中添加元素：

1. 使用赋值语句

可以使用赋值语句来向字典中添加元素。例如，要向字典 `dict1` 中添加一个键为 `'a'`，值为 `1` 的键值对，可以使用以下代码：

```python
dict1 = {}
dict1['a'] = 1
print(dict1)   # 输出 {'a': 1}
```

如果键已经存在，则会覆盖原来的值。例如，对于以下字典：

```python
dict1 = {'a': 1, 'b': 2, 'c': 3}
```

要将键为 `'a'` 的值改为 `4`，可以使用以下代码：

```python
dict1['a'] = 4
print(dict1)   # 输出 {'a': 4, 'b': 2, 'c': 3}
```

2. 使用 update() 方法

可以使用 `update()` 方法向字典中添加多个键值对。例如，要向字典 `dict1` 中添加多个键值对，可以使用以下代码：

```python
dict1 = {'a': 1}
dict1.update({'b': 2, 'c': 3})
print(dict1)   # 输出 {'a': 1, 'b': 2, 'c': 3}
```

如果键已经存在，则会覆盖原来的值。

3. 使用字典解析式

可以使用字典解析式来创建一个新的字典，其中包含需要添加的键值对。例如，要向字典 `dict1` 中添加多个键值对，可以使用以下代码：

```python
dict1 = {'a': 1}
dict2 = {'b': 2, 'c': 3}
dict1 = {**dict1, **dict2}
print(dict1)   # 输出 {'a': 1, 'b': 2, 'c': 3}
```

在这个例子中，我们使用了字典解析式和 `**` 运算符将两个字典合并成一个新的字典，从而向字典 `dict1` 中添加多个键值对。如果键已经存在，则会覆盖原来的值。

需要注意的是，以上方法都会修改原始字典。如果希望在不修改原始字典的情况下添加元素，可以使用 `copy()` 方法复制一个新的字典。例如，要向字典 `dict1` 中添加一个键为 `'a'`，值为 `1` 的键值对，可以使用以下代码：

```python
dict1 = {'b': 2, 'c': 3}
dict2 = dict1.copy()
dict2['a'] = 1
print(dict1)   # 输出 {'b': 2, 'c': 3}
print(dict2)   # 输出 {'b': 2, 'c': 3, 'a': 1}
```

在这个例子中，我们使用了 `copy()` 方法复制了一个新的字典 `dict2`，然后向 `dict2` 中添加了一个键值对，不会修改原始字典 `dict1`。