# 深度学习工程师面试题与答案整理

## 1. Tensor 操作与形状变换

### 1.1 `view`、`reshape`、`flatten` 的区别

**问：** `view`、`reshape`、`flatten` 有什么区别？

**答：**

- `view()`：返回原 tensor 的新视图，**要求内存连续（contiguous）**。
    
- `reshape()`：优先尝试返回 view，如果不行就可能拷贝一份，因此更灵活。
    
- `flatten()`：本质上是把若干连续维度压平，适合直接展平使用。
    

**示例：**

```python
x = torch.randn(2, 3, 4)

a = x.view(2, 12)
b = x.reshape(2, 12)
c = x.flatten(start_dim=1)   # [2, 12]
```

**面试补充：**

- `view` 更“严格”
    
- `reshape` 更“稳妥”
    
- `flatten` 可读性更好，常用于送入全连接层前
    

---

### 1.2 为什么 `permute` 之后直接 `view` 可能报错

**问：** 为什么下面代码可能报错？

```python
x = x.permute(0, 2, 1)
x = x.view(B, -1)
```

**答：**

因为 `permute()` 只改变张量的**维度顺序解释方式**，通常不会真的重排底层内存，所以很多情况下结果 tensor **不是 contiguous**。而 `view()` 要求内存连续，因此会报错。

**正确写法：**

```python
x = x.permute(0, 2, 1).contiguous().view(B, -1)
```

或者直接：

```python
x = x.permute(0, 2, 1).reshape(B, -1)
```

---

### 1.3 什么是 contiguous

**问：** 什么是 contiguous？

**答：**

contiguous 表示 tensor 在内存中的存储是连续的，符合当前 shape 和 stride 的默认布局。

很多操作，比如：

- `transpose`
    
- `permute`
    
- 某些切片操作
    

都会导致 tensor 不连续。

**检查方式：**

```python
x.is_contiguous()
```

---

### 1.4 `transpose` 和 `permute` 的区别

**问：** `transpose` 和 `permute` 有什么区别？

**答：**

- `transpose(dim0, dim1)`：交换两个维度
    
- `permute(*dims)`：按指定顺序重排所有维度
    

**示例：**

```python
x = torch.randn(2, 3, 4)

x1 = x.transpose(1, 2)   # [2, 4, 3]
x2 = x.permute(0, 2, 1)  # [2, 4, 3]
```

二维情况两者很像，但高维张量通常 `permute` 更灵活。

---

### 1.5 `[B, C, H, W]` 转 `[B, H, W, C]`

**问：** 如何把 `[B, C, H, W]` 转成 `[B, H, W, C]`？

**答：**

```python
x = x.permute(0, 2, 3, 1)
```

---

### 1.6 `[B, T, H, D]` 转 `[B, H, T, D]`

**问：** 如何把 `[B, T, H, D]` 转成 `[B, H, T, D]`？

**答：**

```python
x = x.permute(0, 2, 1, 3)
```

这在 multi-head attention 中非常常见。

---

### 1.7 `unsqueeze` 和 `squeeze`

**问：** `unsqueeze` 和 `squeeze` 的作用是什么？

**答：**

- `unsqueeze(dim)`：在指定位置插入一个大小为 1 的维度
    
- `squeeze(dim)`：删除大小为 1 的维度
    

**示例：**

```python
x = torch.randn(2, 3, 4)      # [2, 3, 4]
y = x.unsqueeze(1)            # [2, 1, 3, 4]

z = torch.randn(2, 1, 3, 1)
z = z.squeeze()               # [2, 3]
```

---

### 1.8 `repeat` 和 `expand` 的区别

**问：** `repeat` 和 `expand` 的区别是什么？

**答：**

- `repeat`：**真的复制数据**
    
- `expand`：不复制数据，只是通过 stride 机制“扩展视图”，更省内存
    

**示例：**

```python
x = torch.randn(2, 1, 4)

a = x.repeat(1, 3, 1)   # [2, 3, 4]
b = x.expand(2, 3, 4)   # [2, 3, 4]
```

**注意：**

`expand` 只能扩展原来 size 为 1 的维度。

---

### 1.9 `cat` 和 `stack` 的区别

**问：** `torch.cat` 和 `torch.stack` 的区别是什么？

**答：**

- `cat`：在**已有维度**上拼接
    
- `stack`：在**新维度**上拼接
    

**示例：**

```python
a = torch.randn(2, 3)
b = torch.randn(2, 3)

x = torch.cat([a, b], dim=0)    # [4, 3]
y = torch.stack([a, b], dim=0)  # [2, 2, 3]
```

---

### 1.10 广播机制是什么

**问：** PyTorch 的广播机制是什么？

**答：**

两个 tensor 做逐元素运算时，会从最后一维开始对齐。某一维能广播的条件是：

- 两维相等，或者
    
- 其中一维是 1
    

**示例：**

```python
a = torch.randn(2, 3, 4)
b = torch.randn(1, 3, 4)

c = a + b   # 可以广播
```

**不能广播的例子：**

```python
a = torch.randn(2, 3, 4)
b = torch.randn(2, 2, 4)
# 第二维 3 和 2 不匹配，且都不为 1，不能广播
```

---

### 1.11 如何把 `[B, C, H, W]` 拉平成 `[B, C*H*W]`

**答：**

```python
x = x.reshape(B, -1)
```

或：

```python
x = x.flatten(start_dim=1)
```

---

### 1.12 如何把 `[B, C, H, W]` 变成 `[B, C, H*W]`

**答：**

```python
x = x.reshape(B, C, H * W)
```

或：

```python
x = x.flatten(start_dim=2)
```

---

### 1.13 `[B, T, V]` 的 logits 如何得到预测类别 `[B, T]`

**答：**

```python
pred = logits.argmax(dim=-1)
```

因为最后一维 `V` 表示类别维。

---

### 1.14 `argmax(dim=-1)` 后 shape 怎么变

**答：**

`argmax(dim=-1)` 会沿最后一维取最大值对应下标，因此最后一维会被消掉。

例如：

```python
x.shape = [B, T, V]
x.argmax(dim=-1).shape = [B, T]
```

---

### 1.15 `topk` 返回什么

**问：** `topk` 返回什么？

**答：**

返回两个 tensor：

- values：前 k 大的值
    
- indices：前 k 大值对应的索引
    

**示例：**

```python
values, indices = x.topk(k=3, dim=-1)
```

---

### 1.16 `gather` 是什么，什么时候用

**答：**

`gather` 用于按照索引从指定维度取值，常用于：

- 根据标签取对应类别概率
    
- beam search
    
- attention/select 操作
    

**示例：**

```python
x = torch.tensor([[10, 20, 30], [40, 50, 60]])
idx = torch.tensor([[2], [1]])

y = torch.gather(x, dim=1, index=idx)
# y = [[30], [50]]
```

---

## 2. Autograd 与梯度

### 2.1 `requires_grad=True` 的作用

**问：** `requires_grad=True` 是什么作用？

**答：**

表示该 tensor 需要被 autograd 跟踪，从而在反向传播时计算其梯度。

通常模型参数默认是 `requires_grad=True`。

---

### 2.2 `loss.backward()` 做了什么

**答：**

`loss.backward()` 会从 loss 开始沿计算图反向传播，利用链式法则计算所有需要梯度参数的梯度，并把结果累加到它们的 `.grad` 中。

---

### 2.3 为什么要 `optimizer.zero_grad()`

**问：** 为什么训练时通常先 `zero_grad()`？

**答：**

因为 PyTorch 默认**梯度累加**。如果不清零，上一次 backward 的梯度会和这一次叠加，导致更新错误。

**标准流程：**

```python
optimizer.zero_grad()
loss.backward()
optimizer.step()
```

---

### 2.4 什么是梯度累加

**答：**

梯度累加是指多次 forward/backward 后不立刻更新参数，而是累计若干小 batch 的梯度后再 `step()`，从而模拟更大的 batch size。

**示例：**

```python
accum_steps = 4

for i, (x, y) in enumerate(loader):
    out = model(x)
    loss = criterion(out, y) / accum_steps
    loss.backward()

    if (i + 1) % accum_steps == 0:
        optimizer.step()
        optimizer.zero_grad()
```

---

### 2.5 `model.train()` 和 `model.eval()` 的区别

**答：**

它们主要影响某些层的行为，比如：

- `Dropout`
    
- `BatchNorm`
    

**区别：**

- `model.train()`：训练模式
    
- `model.eval()`：推理/验证模式
    

**例如：**

- Dropout 在 train 时随机失活，在 eval 时关闭
    
- BatchNorm 在 train 时用当前 batch 统计量，在 eval 时用滑动平均统计量
    

---

### 2.6 `torch.no_grad()` 和 `model.eval()` 是一回事吗

**答：**

不是。

- `model.eval()`：切换模型某些层的行为
    
- `torch.no_grad()`：关闭梯度计算，节省显存和计算
    

验证/推理通常两个一起用：

```python
model.eval()
with torch.no_grad():
    out = model(x)
```

---

### 2.7 为什么有的 tensor `.grad` 是 `None`

**答：**

常见原因：

1. 它不是 leaf tensor
    
2. 它的 `requires_grad=False`
    
3. 还没执行 `backward()`
    
4. 计算图中断了，比如用了 `detach()`
    

---

### 2.8 `detach()` 的作用

**答：**

`detach()` 会返回一个新的 tensor，它与原 tensor 共享数据，但**不参与梯度计算**。

**示例：**

```python
y = x.detach()
```

常见用途：

- 推理/记录中间结果
    
- 转 numpy 前切断梯度
    
- 某些自定义训练流程中阻断梯度传播
    

---

## 3. Loss 与输出层匹配

### 3.1 `CrossEntropyLoss` 输入要先 softmax 吗

**答：**

**不要。**

`nn.CrossEntropyLoss` 内部已经包含：

1. `log_softmax`
    
2. `nll_loss`
    

所以传入的应该是**原始 logits**。

**正确：**

```python
logits = model(x)           # [B, num_classes]
loss = criterion(logits, y)
```

**错误：**

```python
loss = criterion(torch.softmax(logits, dim=1), y)
```

---

### 3.2 `BCEWithLogitsLoss` 输入要先 sigmoid 吗

**答：**

**不要。**

`BCEWithLogitsLoss` 内部已经做了 sigmoid，并且数值更稳定。

---

### 3.3 多分类和多标签的区别

**答：**

- **多分类**：一个样本只属于一个类别  
    常用：`CrossEntropyLoss`
    
- **多标签**：一个样本可同时属于多个类别  
    常用：`BCEWithLogitsLoss`
    

---

### 3.4 为什么 `BCEWithLogitsLoss` 比 “sigmoid + BCE” 更稳定

**答：**

因为它把 sigmoid 和二元交叉熵合并在一起实现，避免数值溢出，比如：

- `sigmoid(x)` 极接近 0 或 1
    
- 再做 `log` 时容易出现数值不稳定
    

---

### 3.5 分类任务为什么通常不用 MSE

**答：**

因为 MSE 更适合回归，分类任务更关注概率分布差异，而交叉熵更符合分类目标，梯度性质也更好，收敛通常更快。

---

## 4. 模型结构与核心原理

### 4.1 为什么需要激活函数

**问：** 网络里为什么需要非线性激活函数？

**答：**

如果全是线性层，多层线性变换仍然等价于一层线性变换，模型表达能力很弱。加入非线性激活函数后，网络才能拟合复杂函数。

---

### 4.2 ReLU 有什么优缺点

**答：**

**优点：**

- 计算简单
    
- 缓解 sigmoid/tanh 的梯度消失问题
    
- 收敛通常更快
    

**缺点：**

- 负半轴梯度为 0，可能出现“神经元死亡”
    

---

### 4.3 BatchNorm 和 LayerNorm 的区别

**答：**

**BatchNorm：**

- 对 batch 维做统计
    
- 常见于 CNN
    
- 对 batch size 比较敏感
    

**LayerNorm：**

- 对单个样本的特征维做归一化
    
- 常见于 NLP / Transformer
    
- 不依赖 batch 统计，更适合变长序列和小 batch
    

---

### 4.4 为什么 Transformer 常用 LayerNorm 而不是 BatchNorm

**答：**

因为 Transformer 常处理：

- 变长序列
    
- 小 batch
    
- 自回归解码
    

这时 BatchNorm 依赖 batch 统计不太稳定，而 LayerNorm 只依赖单个样本内部特征，更适合。

---

### 4.5 残差连接为什么有用

**答：**

残差连接能让网络学习“输入到输出的增量”，使梯度更容易传播，缓解深层网络训练困难和梯度消失问题。

公式：

```python
y = F(x) + x
```

---

## 5. CNN 常见面试题

### 5.1 卷积层输出尺寸怎么算

**答：**

二维卷积输出高宽公式：

```text
H_out = floor((H_in + 2P - K) / S) + 1
W_out = floor((W_in + 2P - K) / S) + 1
```

其中：

- `P`：padding
    
- `K`：kernel size
    
- `S`：stride
    

---

### 5.2 卷积层参数量怎么算

**答：**

如果卷积层：

- 输入通道数 `C_in`
    
- 输出通道数 `C_out`
    
- 卷积核大小 `K x K`
    

则参数量为：

```text
C_out * C_in * K * K + C_out   # 如果有 bias
```

---

### 5.3 1x1 卷积有什么作用

**答：**

1x1 卷积常用于：

- 改变通道数
    
- 做通道间信息融合
    
- 降维/升维
    
- 减少计算量
    

---

## 6. Transformer / Attention 高频题

### 6.1 self-attention 是什么

**答：**

self-attention 是让序列中每个位置都能和其他位置交互，根据相关性加权聚合信息。

公式：

```text
Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) V
```

---

### 6.2 为什么要除以 `sqrt(d_k)`

**答：**

如果不缩放，`QK^T` 的值在维度很大时容易变得很大，使 softmax 进入饱和区，梯度变小，训练不稳定。除以 `sqrt(d_k)` 可以让数值更稳定。

---

### 6.3 multi-head attention 的意义

**答：**

多头机制允许模型从不同子空间、不同角度关注信息，提高表达能力。

---

### 6.4 attention 中张量 shape 怎么变化

以输入：

```text
x: [B, T, D]
```

为例，假设有 `H` 个头，每头维度 `d = D / H`

#### 第一步：线性映射得到 Q、K、V

```text
Q, K, V: [B, T, D]
```

#### 第二步：拆成多头

```text
[B, T, D] -> [B, T, H, d]
```

#### 第三步：调整维度顺序

```text
[B, T, H, d] -> [B, H, T, d]
```

#### 第四步：计算注意力分数

```text
Q @ K^T
```

shape：

```text
[B, H, T, d] @ [B, H, d, T] -> [B, H, T, T]
```

#### 第五步：softmax 后乘 V

```text
[B, H, T, T] @ [B, H, T, d] -> [B, H, T, d]
```

#### 第六步：拼回多头

```text
[B, H, T, d] -> [B, T, H, d] -> [B, T, D]
```

---

### 6.5 causal mask 是什么

**答：**

causal mask 用于自回归生成，保证当前位置只能看到自己和前面的位置，不能看到未来 token，防止信息泄露。

---

### 6.6 padding mask 是什么

**答：**

padding mask 用来屏蔽补齐位置，避免模型把 padding token 当成真实信息参与 attention。

---

## 7. Dataset / DataLoader

### 7.1 `Dataset` 和 `DataLoader` 的作用

**答：**

- `Dataset`：定义如何读取单条样本
    
- `DataLoader`：按 batch 组织数据，并支持 shuffle、多进程加载等
    

---

### 7.2 自定义 Dataset 怎么写

```python
from torch.utils.data import Dataset

class MyDataset(Dataset):
    def __init__(self, data, labels):
        self.data = data
        self.labels = labels

    def __len__(self):
        return len(self.data)

    def __getitem__(self, idx):
        x = self.data[idx]
        y = self.labels[idx]
        return x, y
```

---

### 7.3 `num_workers` 是什么

**答：**

表示 DataLoader 使用多少个子进程并行加载数据。

- 太小：数据加载可能跟不上 GPU
    
- 太大：进程切换开销增大，内存占用增加
    

---

### 7.4 `pin_memory=True` 有什么用

**答：**

将 CPU 内存页锁定，能加速数据从 CPU 拷贝到 GPU，尤其是在 CUDA 训练时常有帮助。

---

### 7.5 什么时候需要自定义 `collate_fn`

**答：**

当样本不能直接默认堆叠时，比如：

- 变长序列
    
- 不同尺寸图片
    
- 复杂结构输入
    

---

## 8. 训练循环实战

### 8.1 标准训练循环怎么写

```python
model.train()

for x, y in train_loader:
    x, y = x.to(device), y.to(device)

    optimizer.zero_grad()
    logits = model(x)
    loss = criterion(logits, y)
    loss.backward()
    optimizer.step()
```

---

### 8.2 验证循环怎么写

```python
model.eval()

total_loss = 0
correct = 0

with torch.no_grad():
    for x, y in val_loader:
        x, y = x.to(device), y.to(device)
        logits = model(x)
        loss = criterion(logits, y)
        total_loss += loss.item()

        pred = logits.argmax(dim=1)
        correct += (pred == y).sum().item()
```

---

### 8.3 为什么训练和验证循环不一样

**答：**

验证时：

- 不需要反向传播
    
- 不需要梯度
    
- Dropout / BatchNorm 行为要切到推理模式
    

所以通常要：

```python
model.eval()
with torch.no_grad():
    ...
```

---

## 9. 优化器与训练策略

### 9.1 SGD、Adam、AdamW 的区别

**答：**

**SGD：**

- 简单
    
- 泛化常较好
    
- 收敛可能较慢
    

**Adam：**

- 自适应学习率
    
- 收敛快
    
- 对初始学习率不太敏感
    

**AdamW：**

- 在 Adam 基础上把 weight decay 与梯度更新解耦
    
- 通常比 Adam 更合理，现在线上更常用
    

---

### 9.2 AdamW 为什么比 Adam 更常用

**答：**

因为 Adam 中把 L2 正则直接混到梯度更新里，并不完全等价于真正的 weight decay。AdamW 采用 decoupled weight decay，优化效果通常更好。

---

### 9.3 warmup 为什么有效

**答：**

训练初期参数还不稳定，如果一开始学习率太大，容易震荡甚至发散。warmup 先用较小学习率，再逐步增大，有助于训练稳定，尤其是 Transformer 和大 batch 训练。

---

## 10. 过拟合与欠拟合

### 10.1 怎么判断过拟合

**答：**

常见表现：

- train loss 下降
    
- val loss 上升
    
- train accuracy 很高，但 val/test accuracy 较低
    

---

### 10.2 怎么缓解过拟合

**答：**

常见方法：

- 增加数据量
    
- 数据增强
    
- dropout
    
- weight decay
    
- early stopping
    
- 减小模型容量
    
- label smoothing
    
- mixup / cutmix
    

---

### 10.3 欠拟合是什么

**答：**

模型在训练集上都学不好，表现为：

- train loss 很高
    
- train accuracy 也不高
    

常见原因：

- 模型太简单
    
- 训练不够久
    
- 学习率不合适
    
- 特征不足
    

---

## 11. Debug 高频题

### 11.1 loss 一直不下降怎么排查

**答：**

可以从以下几个方向排查：

1. 学习率是否过大或过小
    
2. 数据和标签是否对齐
    
3. 最后一层输出与 loss 是否匹配
    
4. 是否忘了 `model.train()`
    
5. 是否忘了 `optimizer.step()`
    
6. 梯度是否为 0
    
7. 参数是否传给了 optimizer
    
8. 是否错误使用了 `detach()`
    
9. 输入归一化是否合理
    
10. 先尝试在一个很小的数据子集上过拟合，验证代码链路是否正确
    

---

### 11.2 loss 变成 NaN 怎么排查

**答：**

常见原因：

1. 学习率过大
    
2. 输入数据有 NaN / Inf
    
3. 出现除零
    
4. `log(0)`
    
5. softmax 前值过大
    
6. 混合精度数值不稳定
    
7. 梯度爆炸
    
8. 自定义 loss 写错
    

**排查思路：**

```python
torch.isnan(x).any()
torch.isinf(x).any()
```

并检查：

- 输入
    
- 中间层输出
    
- loss
    
- 梯度
    

---

### 11.3 显存 OOM 怎么处理

**答：**

常见办法：

- 减小 batch size
    
- 降低输入分辨率或序列长度
    
- 使用混合精度
    
- gradient checkpointing
    
- 梯度累加
    
- 删除无用变量
    
- 验证时加 `torch.no_grad()`
    
- 冻结部分参数
    
- 用 LoRA 等高效微调
    

---

### 11.4 GPU 利用率低怎么排查

**答：**

常见原因：

- 数据加载太慢
    
- `num_workers` 太小
    
- CPU 预处理太重
    
- batch 太小
    
- 频繁同步操作
    
- 日志打印过多
    
- 磁盘 IO 慢
    

先看是不是 DataLoader 瓶颈，再看模型计算是否太轻。

---

## 12. 混合精度训练

### 12.1 混合精度为什么更快、更省显存

**答：**

因为部分计算和激活可以用 `float16/bfloat16` 表示：

- 显存占用更小
    
- Tensor Core 计算更快
    

---

### 12.2 为什么混合精度可能不稳定

**答：**

因为 `float16` 表示范围更小，容易出现：

- 下溢
    
- 上溢
    
- 梯度变成 0 或 NaN
    

所以常配合 `GradScaler` 使用。

---

### 12.3 PyTorch 混合精度基本写法

```python
scaler = torch.cuda.amp.GradScaler()

for x, y in loader:
    optimizer.zero_grad()

    with torch.cuda.amp.autocast():
        logits = model(x)
        loss = criterion(logits, y)

    scaler.scale(loss).backward()
    scaler.step(optimizer)
    scaler.update()
```

---

## 13. 手写题模板

### 13.1 手写 `[B, C, H, W] -> [B, H, W, C]`

```python
x = x.permute(0, 2, 3, 1)
```

---

### 13.2 手写 `[B, T, D] -> [B, 1, T, D]`

```python
x = x.unsqueeze(1)
```

---

### 13.3 手写 `[B, 1, T, 1, D] -> [B, T, D]`

```python
x = x.squeeze(1).squeeze(2)
```

或：

```python
x = x.squeeze()
```

如果确定只会去掉想要的 size=1 维度，可以用 `squeeze()`；更稳妥的是指定维度。

---

### 13.4 手写 `[B, T, D] -> [B*T, D]`

```python
x = x.reshape(-1, D)
```

---

### 13.5 手写取最后一个时间步 `[B, T, D] -> [B, D]`

```python
x_last = x[:, -1, :]
```

---

### 13.6 手写分类预测

```python
pred = logits.argmax(dim=1)
```

如果 `logits.shape = [B, num_classes]`

---

## 14. 高频“为什么”速答

### 14.1 为什么 `view` 比 `reshape` 更容易报错

因为 `view` 要求底层内存连续，而 `reshape` 会在必要时做拷贝。

---

### 14.2 为什么 attention 要除以 `sqrt(d_k)`

避免点积值过大导致 softmax 饱和，从而梯度太小。

---

### 14.3 为什么 LayerNorm 更适合 Transformer

因为它不依赖 batch 统计，更适合变长序列、小 batch 和自回归场景。

---

### 14.4 为什么 residual 能缓解深层网络训练困难

因为它让梯度更容易跨层传播，并让网络学习残差而不是完整映射。

---

### 14.5 为什么 `CrossEntropyLoss` 不需要提前 softmax

因为它内部已经做了 `log_softmax`，提前做 softmax 反而会影响数值稳定性。

---

## 15. 最值得优先背熟的 20 题

1. `view` vs `reshape`
    
2. `transpose` vs `permute`
    
3. `squeeze` vs `unsqueeze`
    
4. `repeat` vs `expand`
    
5. `cat` vs `stack`
    
6. contiguous 是什么
    
7. 广播机制
    
8. `argmax/topk/gather`
    
9. `requires_grad` 是什么
    
10. `loss.backward()` 做了什么
    
11. 为什么要 `zero_grad`
    
12. `train()` vs `eval()`
    
13. `no_grad()` vs `eval()`
    
14. CrossEntropyLoss 输入要求
    
15. BCEWithLogitsLoss 输入要求
    
16. BatchNorm vs LayerNorm
    
17. attention shape 推导
    
18. loss 不下降怎么排查
    
19. loss NaN 怎么排查
    
20. OOM 怎么处理
    

---

## 16. 一段适合面试口述的总结

深度学习工程师面试里，最常考的不只是模型原理，还会非常关注实际写代码和排障能力。  
比如 Tensor 的转置、reshape、permute、广播、cat/stack 这些基础操作，必须非常熟。  
再往上，面试官常会追问 autograd、loss 与输出层匹配、训练循环、DataLoader、模型调参、显存和 NaN 排查。  
如果是中高级岗位，还会进一步考 Transformer、分布式训练、混合精度、部署优化和线上问题定位。

---

如果你愿意，我下一条可以继续直接给你第二部分，格式保持一致：

**《深度学习工程师面试题与答案整理（2）：CNN / RNN / Transformer / LLM / 部署 / Debug 进阶版》**

好，下面继续，保持成套的 Markdown 笔记风格。你可以直接接在上一份后面。

---

# 深度学习工程师面试题与答案整理（2）

## 17. CNN 高频面试题与答案

### 17.1 卷积和全连接有什么区别

**问：** 卷积层和全连接层有什么区别？

**答：**

**卷积层：**

- 局部连接
    
- 权重共享
    
- 更适合处理图像这类有空间结构的数据
    
- 参数量相对更少
    

**全连接层：**

- 每个输出神经元和所有输入相连
    
- 参数量通常更大
    
- 更适合做最终分类或特征融合
    

**面试补充：**  
卷积利用了图像的局部相关性和平移不变性，因此比直接用全连接处理图像更高效。

---

### 17.2 卷积输出尺寸怎么算

**问：** 给定输入尺寸、卷积核、padding、stride，怎么计算输出尺寸？

**答：**

二维卷积输出公式：

```text
H_out = floor((H_in + 2P - K) / S) + 1
W_out = floor((W_in + 2P - K) / S) + 1
```

其中：

- `H_in, W_in`：输入高宽
    
- `K`：卷积核大小
    
- `P`：padding
    
- `S`：stride
    

**例子：**  
输入 `32x32`，卷积核 `3x3`，padding=1，stride=1：

```text
输出 = 32x32
```

---

### 17.3 卷积层参数量怎么算

**问：** 一个卷积层参数量怎么计算？

**答：**

如果：

- 输入通道数 `C_in`
    
- 输出通道数 `C_out`
    
- 卷积核大小 `K x K`
    

则参数量为：

```text
C_out * C_in * K * K + C_out
```

如果没有 bias，就去掉最后的 `+ C_out`。

**例子：**  
输入通道 3，输出通道 64，卷积核 3x3：

```text
64 * 3 * 3 * 3 + 64 = 1792
```

---

### 17.4 1x1 卷积有什么作用

**答：**

1x1 卷积常用于：

1. 调整通道数
    
2. 做跨通道信息融合
    
3. 降低计算量
    
4. 用在 bottleneck 结构中压缩/恢复通道
    

**面试话术：**  
1x1 卷积虽然不看空间邻域，但能在线性组合通道特征方面非常有效。

---

### 17.5 pooling 层有什么作用

**答：**

池化层主要作用：

- 降低特征图尺寸
    
- 减少计算量
    
- 增大感受野
    
- 提升一定的平移鲁棒性
    

**常见类型：**

- Max Pooling：保留最强响应
    
- Avg Pooling：保留平均信息
    

---

### 17.6 Max Pooling 和 Avg Pooling 的区别

**答：**

- `Max Pooling`：取局部区域最大值，更突出显著特征
    
- `Avg Pooling`：取局部区域平均值，更平滑
    

**经验上：**

- CNN 主干里常见 Max Pooling
    
- 分类头尾部常见 Global Average Pooling
    

---

### 17.7 什么是感受野

**答：**

感受野是指某个神经元在输入图像上“能看到”的区域大小。

**面试补充：**  
深层网络、较大卷积核、stride、pooling 都会增大感受野。感受野越大，模型越能利用全局上下文。

---

### 17.8 same padding 和 valid padding 的区别

**答：**

- `same padding`：输出尺寸尽量和输入一致
    
- `valid padding`：不补零，输出尺寸通常变小
    

---

### 17.9 深度可分离卷积是什么

**答：**

深度可分离卷积把标准卷积分成两步：

1. **Depthwise Conv**：每个输入通道单独做卷积
    
2. **Pointwise Conv**：再用 1x1 卷积融合通道
    

**优点：**

- 参数更少
    
- 计算更少
    

**典型模型：**

- MobileNet
    

---

### 17.10 group convolution 是什么

**答：**

group convolution 是把输入通道分组，每组分别卷积。

**优点：**

- 降低计算量
    
- 支持更灵活的特征学习
    

**典型：**

- ResNeXt
    

---

### 17.11 为什么 ResNet 有效

**答：**

ResNet 的核心是残差连接：

```text
y = F(x) + x
```

它有效的原因：

1. 缓解梯度消失
    
2. 让深层网络更容易优化
    
3. 允许网络学习残差而不是完整映射
    

---

### 17.12 什么是 bottleneck 结构

**答：**

ResNet 的 bottleneck 一般是：

```text
1x1 -> 3x3 -> 1x1
```

作用：

- 第一个 1x1 降维
    
- 3x3 提取空间特征
    
- 最后一个 1x1 升维
    

这样能减少计算量。

---

### 17.13 空洞卷积是什么

**答：**

空洞卷积（dilated convolution）是在卷积核元素之间插入空洞，从而在不增加参数量太多的情况下扩大感受野。

**适用场景：**

- 语义分割
    
- 需要大感受野但不想过度下采样的任务
    

---

### 17.14 反卷积是什么，有什么问题

**答：**

反卷积通常指转置卷积（transposed convolution），常用于上采样。

**问题：**

- 容易产生 checkerboard artifacts（棋盘格伪影）
    

**替代方案：**

- 双线性插值 + 卷积
    
- 最近邻上采样 + 卷积
    

---

## 18. RNN / LSTM / GRU 高频题与答案

### 18.1 RNN 的核心问题是什么

**答：**

RNN 的核心问题是长序列训练时容易出现：

- 梯度消失
    
- 梯度爆炸
    

因此很难建模长距离依赖。

---

### 18.2 LSTM 为什么比普通 RNN 更强

**答：**

LSTM 引入了门控机制：

- 输入门
    
- 遗忘门
    
- 输出门
    

并维护一个 cell state，可以更好地保留长期信息，因此更适合长序列建模。

---

### 18.3 GRU 和 LSTM 的区别

**答：**

**GRU：**

- 结构更简单
    
- 参数更少
    
- 训练更快
    

**LSTM：**

- 表达能力更强
    
- 有独立的 cell state
    
- 对复杂长依赖有时更好
    

---

### 18.4 hidden state 和 cell state 的区别

**答：**

- `hidden state (h)`：当前时刻的输出表示
    
- `cell state (c)`：长期记忆通道
    

LSTM 通过 cell state 来缓解梯度消失问题。

---

### 18.5 双向 RNN 有什么优缺点

**答：**

**优点：**

- 同时利用前向和后向上下文
    
- 对很多理解任务更有效
    

**缺点：**

- 不能直接用于严格自回归生成
    
- 计算量更大
    

---

### 18.6 为什么 RNN 不如 Transformer 常用

**答：**

主要原因：

1. RNN 难以并行
    
2. 长距离依赖建模弱
    
3. 训练慢
    
4. Transformer 在大规模数据上效果更好
    

但在一些小模型、流式处理、低资源场景下，RNN 仍然有用。

---

### 18.7 变长序列如何处理

**答：**

常见方法：

1. padding 到同一长度
    
2. 使用 attention mask
    
3. 对 RNN 可用 `pack_padded_sequence`
    

---

### 18.8 teacher forcing 是什么

**答：**

在 seq2seq 训练中，decoder 当前步输入使用真实标签，而不是上一步模型预测结果，这就叫 teacher forcing。

**优点：**

- 训练更稳定
    
- 收敛更快
    

**问题：**

- 训练和推理不一致，可能产生 exposure bias
    

---

## 19. Transformer 高频题与答案

### 19.1 Transformer 的整体结构是什么

**答：**

一个标准 Transformer block 一般包括：

1. Multi-Head Attention
    
2. Add & Norm
    
3. Feed Forward Network
    
4. Add & Norm
    

Encoder 和 Decoder 会有些差别，Decoder 还会包含 masked self-attention 和 cross-attention。

---

### 19.2 self-attention、cross-attention 区别

**答：**

**self-attention：**  
Q、K、V 来自同一个序列。

**cross-attention：**  
Q 来自当前序列，K、V 来自另一个序列。

**应用：**

- Encoder 内部通常是 self-attention
    
- Encoder-Decoder 模型中 decoder 会用 cross-attention
    

---

### 19.3 Multi-Head Attention 为什么有用

**答：**

因为不同 head 可以关注不同类型的关系，比如：

- 局部关系
    
- 长距离依赖
    
- 不同语义模式
    

这样模型表达能力更强。

---

### 19.4 Feed Forward Network 在 Transformer 中做什么

**答：**

FFN 通常对每个 token 独立地做两层 MLP 变换，增强非线性表达能力。

典型形式：

```text
FFN(x) = W2 * activation(W1 * x + b1) + b2
```

---

### 19.5 pre-norm 和 post-norm 的区别

**答：**

**post-norm：**  
先子层，再残差，再 LayerNorm。

**pre-norm：**  
先 LayerNorm，再子层，再残差。

**经验：**  
pre-norm 在深层 Transformer 中训练更稳定，因此现代大模型更常用。

---

### 19.6 positional encoding 为什么需要

**答：**

因为 self-attention 本身不包含顺序信息。  
如果不加入位置信息，模型无法区分 token 顺序。

---

### 19.7 绝对位置编码和相对位置编码区别

**答：**

**绝对位置编码：**  
直接给每个位置一个固定或可学习的 embedding。

**相对位置编码：**  
关注位置之间的相对距离。

**相对位置编码的优点：**

- 更容易泛化到不同长度
    
- 更符合很多序列关系建模方式
    

---

### 19.8 RoPE 是什么

**答：**

RoPE（Rotary Position Embedding）是一种将位置信息通过旋转方式融入 Q/K 的位置编码方法。

**优点：**

- 自然编码相对位置信息
    
- 在 LLM 中表现很好
    
- 长上下文扩展性相对更强
    

---

### 19.9 为什么 Transformer 复杂度是 `O(n^2)`

**答：**

因为 attention 需要计算序列中任意两个位置之间的相关性，得到一个 `n x n` 的注意力矩阵，所以时间和显存复杂度都和 `n^2` 相关。

---

### 19.10 长序列怎么优化 attention

**答：**

常见方法：

- 稀疏注意力
    
- 线性注意力
    
- Flash Attention
    
- 分块注意力
    
- 滑动窗口注意力
    
- KV cache（推理阶段）
    

---

### 19.11 Flash Attention 为什么快

**答：**

Flash Attention 的核心不是改公式，而是通过更高效的 IO 和 kernel fusion，减少显存读写开销，从而提升速度并降低显存占用。

---

### 19.12 KV cache 是什么

**答：**

在自回归推理时，历史 token 的 K 和 V 不需要每一步都重新计算，可以缓存起来，后续只计算新 token 的 Q 与缓存的 K/V 交互。

**作用：**

- 显著提升生成速度
    

---

### 19.13 为什么解码阶段比预填充阶段慢

**答：**

因为：

- Prefill 阶段可以对整段输入并行计算
    
- Decoding 阶段每次只生成一个 token，天然串行
    

因此 token-by-token 解码通常吞吐更低。

---

## 20. LLM 高频题与答案

### 20.1 大语言模型训练目标是什么

**答：**

最常见的是 next token prediction，即根据前面的上下文预测下一个 token。

---

### 20.2 为什么推理时只取最后一个 token 的 logits

**答：**

因为自回归生成的目标是预测“下一个 token”，所以只关心当前位置最后一步的输出分布。

---

### 20.3 temperature、top-k、top-p 是什么

**答：**

**temperature：**  
控制分布平滑程度。

- 高：更随机
    
- 低：更确定
    

**top-k：**  
只在概率最高的前 k 个 token 中采样。

**top-p：**  
选取累计概率达到 p 的最小 token 集合中采样。

---

### 20.4 beam search 和 sampling 的区别

**答：**

**beam search：**

- 更偏向寻找高概率序列
    
- 结果更稳定
    
- 容易模式单一
    

**sampling：**

- 多样性更高
    
- 更适合开放生成
    

---

### 20.5 为什么 LLM 推理显存大

**答：**

主要来自：

1. 模型参数
    
2. KV cache
    
3. 激活和中间缓冲
    
4. batch 和上下文长度增加
    

长上下文场景下，KV cache 占用尤其明显。

---

### 20.6 LoRA 是什么

**答：**

LoRA 是一种参数高效微调方法。它不直接更新原始大权重矩阵，而是在旁边加一个低秩分解的可训练增量矩阵。

**优点：**

- 可训练参数少
    
- 显存占用低
    
- 微调速度快
    

---

### 20.7 为什么 LoRA 能减少训练开销

**答：**

因为它冻结了大部分原模型参数，只训练少量低秩矩阵，所以：

- 显存更省
    
- 反向传播更轻
    
- 存储多个任务适配器更方便
    

---

### 20.8 SFT、RLHF、DPO 分别是什么

**答：**

**SFT（Supervised Fine-Tuning）：**  
用高质量指令数据监督微调。

**RLHF：**  
先训练奖励模型，再通过强化学习优化模型输出偏好。

**DPO：**  
不显式训练 RL 过程，直接利用偏好对做优化，训练更简单。

---

### 20.9 RAG 的基本流程是什么

**答：**

RAG 一般流程：

1. 用户提问
    
2. query 改写/向量化
    
3. 检索相关文档
    
4. rerank（可选）
    
5. 把检索内容拼到 prompt
    
6. LLM 生成答案
    

---

### 20.10 RAG 为什么不能完全解决幻觉

**答：**

因为即使检索到了相关文档，模型仍可能：

- 错误理解文档
    
- 错误引用
    
- 自行编造补全内容
    

RAG 能降低幻觉，但不能彻底消除。

---

### 20.11 embedding 模型和生成模型区别

**答：**

**embedding 模型：**  
输出向量表示，用于检索、聚类、匹配等。

**生成模型：**  
输出文本 token 概率分布，用于续写、问答、对话等。

---

### 20.12 reranker 是什么

**答：**

reranker 是在召回后对候选文档进行更精细排序的模型。

**作用：**

- 提高最终送给 LLM 的上下文质量
    
- 改善检索准确率
    

---

## 21. Loss / 指标 高频题与答案

### 21.1 accuracy、precision、recall、F1 的区别

**答：**

**accuracy：**  
整体预测正确比例。

**precision：**  
预测为正的样本中，真正为正的比例。

**recall：**  
真实为正的样本中，被找出来的比例。

**F1：**  
precision 和 recall 的调和平均。

```text
F1 = 2PR / (P + R)
```

---

### 21.2 类别不平衡时为什么 accuracy 不可靠

**答：**

因为如果负样本占 99%，模型全预测负类也能有 99% accuracy，但实际上毫无价值。

此时更应关注：

- precision
    
- recall
    
- F1
    
- PR-AUC
    

---

### 21.3 ROC-AUC 和 PR-AUC 有什么区别

**答：**

**ROC-AUC：**  
看整体区分正负类能力。

**PR-AUC：**  
更关注正类识别效果，在类别极不平衡时更有参考价值。

---

### 21.4 IoU 是什么

**答：**

IoU（Intersection over Union）是预测框与真实框交并比：

```text
IoU = 交集面积 / 并集面积
```

常用于目标检测和分割。

---

### 21.5 mAP 是什么

**答：**

mAP 是 mean Average Precision，常用于目标检测，表示多个类别上的平均 AP。

---

### 21.6 perplexity 是什么

**答：**

perplexity（困惑度）常用于语言模型，衡量模型对文本序列预测的不确定性。

**越低越好。**

它本质上和交叉熵相关。

---

## 22. 优化器与训练策略 高频题

### 22.1 weight decay 和 L2 正则完全一样吗

**答：**

在普通 SGD 下它们比较接近，但在 Adam 这类自适应优化器中并不完全等价。  
因此 AdamW 使用 decoupled weight decay，更合理。

---

### 22.2 batch size 变大后学习率怎么办

**答：**

经验上常使用线性缩放规则：batch size 变大若干倍，学习率也相应增大。  
但实际中通常还要配合 warmup。

---

### 22.3 大 batch 训练有什么问题

**答：**

可能问题：

- 泛化变差
    
- 容易落入 sharp minima
    
- 初期训练不稳定
    
- 对学习率和 warmup 更敏感
    

---

### 22.4 EMA 参数是什么

**答：**

EMA（Exponential Moving Average）是对模型参数做指数滑动平均。

**作用：**

- 让参数更平滑
    
- 常能提升验证和推理效果
    

---

### 22.5 冻结部分层训练有什么好处

**答：**

- 减少训练开销
    
- 防止小数据集过拟合
    
- 保留预训练模型底层通用特征
    
- 适合迁移学习
    

---

## 23. 过拟合 / 欠拟合 / 泛化 高频题

### 23.1 训练 loss 降、验证 loss 升说明什么

**答：**

通常说明模型开始过拟合训练集了。

---

### 23.2 dropout 为什么能缓解过拟合

**答：**

Dropout 训练时随机屏蔽部分神经元，相当于对网络做随机子模型采样，减少神经元共适应，提升泛化能力。

---

### 23.3 数据增强为什么有效

**答：**

因为它相当于扩展训练数据分布，让模型学习到更稳健、更不依赖特定表面模式的特征。

---

### 23.4 label smoothing 为什么能提升泛化

**答：**

label smoothing 不让模型把正确类别概率学得过于极端，从而减少过拟合，提高校准性。

---

## 24. Debug / 排障 高频题与答案

### 24.1 如何系统排查 loss 不下降

**答：**

建议按顺序查：

1. 看数据是否正确
    
2. 看标签是否对齐
    
3. 看 loss 和输出层是否匹配
    
4. 看模型参数是否参与优化
    
5. 看梯度是否正常
    
6. 尝试在一个很小的数据集上过拟合
    
7. 调学习率
    
8. 检查归一化、初始化、激活函数
    

**非常重要的一句：**  
如果模型连很小的数据子集都无法过拟合，通常说明实现有 bug。

---

### 24.2 如何排查梯度为 0

**答：**

常见原因：

- 学习率太小
    
- 激活函数饱和
    
- 参数被冻结
    
- 使用了 `detach()`
    
- loss 写错
    
- 梯度被清空时机错误
    
- 死亡 ReLU
    

可以打印：

```python
for name, p in model.named_parameters():
    if p.grad is not None:
        print(name, p.grad.abs().mean())
```

---

### 24.3 如何排查 NaN

**答：**

排查顺序：

1. 输入数据是否 NaN/Inf
    
2. 中间层输出是否爆炸
    
3. loss 前有没有除零/开方/log
    
4. 学习率是否过大
    
5. fp16 是否不稳定
    
6. 是否发生梯度爆炸
    

常用检查：

```python
torch.isnan(x).any()
torch.isinf(x).any()
```

---

### 24.4 gradient clipping 是什么

**答：**

梯度裁剪是在梯度过大时把它限制在某个范围内，防止梯度爆炸。

**常见写法：**

```python
torch.nn.utils.clip_grad_norm_(model.parameters(), max_norm=1.0)
```

---

### 24.5 训练很慢怎么排查

**答：**

看四类问题：

1. **数据层**
    
    - DataLoader 太慢
        
    - num_workers 不合理
        
    - IO 慢
        
2. **计算层**
    
    - batch 太小
        
    - GPU 没吃满
        
    - 模型 kernel 不高效
        
3. **同步层**
    
    - CPU/GPU 频繁同步
        
    - 多卡通信开销大
        
4. **工程层**
    
    - 日志太多
        
    - 验证太频繁
        
    - 不必要的数据拷贝
        

---

### 24.6 为什么 GPU 利用率低但显存占用很高

**答：**

说明模型/数据已经把显存占住了，但计算没有持续喂满 GPU。  
常见原因：

- DataLoader 跟不上
    
- batch 太小
    
- 模型里存在串行瓶颈
    
- CPU 预处理太重
    

---

## 25. 分布式训练 / 工程化 高频题

### 25.1 DataParallel 和 DDP 区别

**答：**

**DataParallel（DP）：**

- 单进程多卡
    
- 实现简单
    
- 性能较差
    

**DistributedDataParallel（DDP）：**

- 多进程多卡
    
- 通信更高效
    
- 扩展性更强
    
- 现在更常用
    

---

### 25.2 为什么 DDP 比 DP 更常用

**答：**

因为 DDP：

- 避免单进程瓶颈
    
- 梯度同步更高效
    
- 性能和扩展性更好
    
- 工业训练中基本是主流方案
    

---

### 25.3 随机种子设了为什么还不能完全复现

**答：**

因为还可能存在：

- CUDA 非确定性算子
    
- 多线程调度差异
    
- 数据加载顺序细微波动
    
- 不同硬件或库版本差异
    

所以“尽量复现”容易，“绝对复现”较难。

---

### 25.4 checkpoint 需要保存什么

**答：**

完整断点恢复通常要保存：

- `model.state_dict()`
    
- `optimizer.state_dict()`
    
- `scheduler.state_dict()`
    
- 当前 epoch / global step
    
- 最优指标
    
- 混合精度 scaler（如果有）
    

---

## 26. 部署与上线 高频题

### 26.1 模型部署要关注什么

**答：**

部署通常关注：

1. 延迟
    
2. 吞吐
    
3. 显存/内存占用
    
4. 稳定性
    
5. 可扩展性
    
6. 监控和回滚能力
    

---

### 26.2 ONNX、TensorRT 是什么

**答：**

**ONNX：**  
一个模型交换格式，便于跨框架部署。

**TensorRT：**  
NVIDIA 的高性能推理优化框架，可进行：

- 图优化
    
- kernel fusion
    
- 量化
    
- 加速推理
    

---

### 26.3 量化、剪枝、蒸馏分别是什么

**答：**

**量化：**  
把参数/激活从 fp32 变成 int8/int4 等，减少显存和加速推理。

**剪枝：**  
去掉不重要参数或通道，减少计算。

**蒸馏：**  
用大模型指导小模型训练，提高小模型性能。

---

### 26.4 离线指标高、线上效果差可能为什么

**答：**

常见原因：

1. 训练数据与线上分布不一致
    
2. 离线指标和业务目标不一致
    
3. 线上特征缺失或漂移
    
4. 推理链路和训练链路不一致
    
5. 延迟约束导致线上使用简化版本
    
6. 用户行为反馈闭环变化
    

---

### 26.5 数据漂移是什么

**答：**

数据漂移是指线上输入分布与训练时分布发生变化，导致模型性能下降。

常见包括：

- 特征分布漂移
    
- 标签分布漂移
    
- 概念漂移
    

---

## 27. 开放式系统设计题

### 27.1 设计一个训练到上线的完整流程，你会怎么答

**答：**

可以按这条主线回答：

1. 数据采集与清洗
    
2. 训练集/验证集/测试集划分
    
3. 特征处理或 tokenizer 流程
    
4. 模型训练与实验管理
    
5. 指标评估与错误分析
    
6. 模型压缩/导出
    
7. 部署到推理服务
    
8. 线上监控
    
9. A/B 测试
    
10. 回滚与持续迭代
    

**面试建议：**  
回答时尽量加入“监控、版本管理、回滚、数据漂移”这些关键词，会显得更工程化。

---

### 27.2 设计一个大模型推理服务要考虑什么

**答：**

重点考虑：

- 模型并行/张量并行
    
- KV cache 管理
    
- continuous batching
    
- prefix cache
    
- 延迟与吞吐平衡
    
- 长上下文显存控制
    
- 量化
    
- 多租户隔离
    
- 监控与限流
    

---

### 27.3 如何设计一个 RAG 系统

**答：**

典型模块：

1. 文档切分与清洗
    
2. embedding 建库
    
3. 查询改写
    
4. 向量召回
    
5. rerank
    
6. prompt 组装
    
7. LLM 生成
    
8. 引用与可解释性
    
9. 监控召回率和答案质量
    

---

## 28. 高频手写代码题模板

### 28.1 手写一个两层 MLP

```python
import torch
import torch.nn as nn

class MLP(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim):
        super().__init__()
        self.net = nn.Sequential(
            nn.Linear(in_dim, hidden_dim),
            nn.ReLU(),
            nn.Linear(hidden_dim, out_dim)
        )

    def forward(self, x):
        return self.net(x)
```

---

### 28.2 手写一个残差块

```python
import torch.nn as nn

class ResidualBlock(nn.Module):
    def __init__(self, dim):
        super().__init__()
        self.block = nn.Sequential(
            nn.Linear(dim, dim),
            nn.ReLU(),
            nn.Linear(dim, dim)
        )

    def forward(self, x):
        return x + self.block(x)
```

---

### 28.3 手写 scaled dot-product attention

```python
import math
import torch

def attention(Q, K, V, mask=None):
    # Q, K, V: [B, H, T, D]
    scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(Q.size(-1))

    if mask is not None:
        scores = scores.masked_fill(mask == 0, float("-inf"))

    weights = torch.softmax(scores, dim=-1)
    out = torch.matmul(weights, V)
    return out, weights
```

---

### 28.4 手写 Multi-Head Attention 简化版

```python
import torch
import torch.nn as nn
import math

class MultiHeadAttention(nn.Module):
    def __init__(self, dim, num_heads):
        super().__init__()
        assert dim % num_heads == 0
        self.dim = dim
        self.num_heads = num_heads
        self.head_dim = dim // num_heads

        self.q_proj = nn.Linear(dim, dim)
        self.k_proj = nn.Linear(dim, dim)
        self.v_proj = nn.Linear(dim, dim)
        self.out_proj = nn.Linear(dim, dim)

    def forward(self, x, mask=None):
        B, T, D = x.shape

        Q = self.q_proj(x).view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        K = self.k_proj(x).view(B, T, self.num_heads, self.head_dim).transpose(1, 2)
        V = self.v_proj(x).view(B, T, self.num_heads, self.head_dim).transpose(1, 2)

        scores = torch.matmul(Q, K.transpose(-2, -1)) / math.sqrt(self.head_dim)

        if mask is not None:
            scores = scores.masked_fill(mask == 0, float("-inf"))

        attn = torch.softmax(scores, dim=-1)
        out = torch.matmul(attn, V)   # [B, H, T, head_dim]

        out = out.transpose(1, 2).contiguous().view(B, T, D)
        out = self.out_proj(out)
        return out
```

---

### 28.5 手写 LayerNorm

```python
import torch
import torch.nn as nn

class MyLayerNorm(nn.Module):
    def __init__(self, dim, eps=1e-5):
        super().__init__()
        self.gamma = nn.Parameter(torch.ones(dim))
        self.beta = nn.Parameter(torch.zeros(dim))
        self.eps = eps

    def forward(self, x):
        mean = x.mean(dim=-1, keepdim=True)
        var = x.var(dim=-1, unbiased=False, keepdim=True)
        x_hat = (x - mean) / torch.sqrt(var + self.eps)
        return self.gamma * x_hat + self.beta
```

---

## 29. 高频“为什么”速答补充

### 29.1 为什么 Adam 收敛快但有时泛化不如 SGD

**答：**

因为 Adam 的自适应学习率让优化过程更快，但有时会更快进入某些泛化不佳的解；而 SGD 带噪声的更新方式有时更容易找到泛化更好的平坦极小值。

---

### 29.2 为什么 BatchNorm 对 batch size 敏感

**答：**

因为它依赖 batch 内统计量。batch 很小时，均值和方差估计不稳定，训练效果可能变差。

---

### 29.3 为什么 gradient checkpointing 能省显存但更慢

**答：**

因为它在前向时不保存全部中间激活，而是在反向传播时重新计算部分前向过程。  
所以：

- 显存更省
    
- 计算更多
    
- 训练更慢
    

---

### 29.4 为什么大模型里数据质量很重要

**答：**

因为模型再大，如果数据噪声高、重复多、分布差，模型学到的模式也会受限。很多时候高质量数据比单纯加参数更有效。

---

## 30. 最值得背熟的进阶 20 题

1. 卷积输出尺寸公式
    
2. 卷积参数量计算
    
3. 1x1 卷积作用
    
4. ResNet 为什么有效
    
5. LSTM 比 RNN 强在哪里
    
6. GRU 和 LSTM 区别
    
7. self-attention 公式
    
8. 为什么除以 `sqrt(d_k)`
    
9. Transformer 中各步 shape
    
10. pre-norm vs post-norm
    
11. positional encoding 为什么需要
    
12. RoPE 是什么
    
13. KV cache 是什么
    
14. LoRA 原理
    
15. RAG 流程
    
16. accuracy / precision / recall / F1
    
17. 类别不平衡为什么不能只看 accuracy
    
18. DDP vs DP
    
19. 离线高线上差怎么排查
    
20. 大模型推理服务设计关注点
    

---

## 31. 一段适合面试口述的进阶总结

如果是中高级深度学习岗位，面试通常不会只停留在 tensor 操作和训练循环，还会进一步考察你对 CNN、RNN、Transformer、LLM、优化策略、分布式训练、部署和线上问题的理解。  
其中 Transformer 和 LLM 现在几乎是必考，尤其是 attention shape、KV cache、LoRA、RAG、推理优化这些内容。  
如果再往工程方向走，面试官还会看你有没有系统地排查过 NaN、OOM、GPU 利用率低、数据漂移、离线线上不一致等真实问题。

---

下一部分我建议直接给你最有用的一版：

**《深度学习工程师面试题与答案整理（3）：100 道高频面试题速答版》**

这一版会更适合你最后冲刺背诵。