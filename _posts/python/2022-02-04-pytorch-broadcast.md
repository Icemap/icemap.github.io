---
layout: single
title:  "pytorch 广播机制"
date:   2022-02-04 16:20:51 +0800
categories:
  - python grammer
tags:
  - 语法
---

# pytorch 广播机制

- pytorch的广播机制，本质上就是利用运算符重载，针对原本不支持计算的一种情况: `不同形状的向量的互操作`进行了拓展
- 这种拓展提高了性能，因为可以一次性将变量提交至C/C++语言层，减少了Python和C/C++的交互
- 这种拓展提高了代码的简洁程度，一定程度上减少了程序员的心智负担
- 但也提高了出错的可能性，因为广播的存在，将fast-failed机制破坏了
- 因此需仔细研究广播机制，以免过多的编码错误

## 满足广播机制的条件

- 两张量形状不同
- 形状不同的维度，至少有一方的维度为1


# 举几个🌰

## 1. 非广播机制

- 单纯就是两个张量的对应数字直接操作（相乘）

```python
import torch

print("normal calculator")
tensor_a = torch.tensor([[1, 2, 3], [4, 5, 6]])
print(f"tensor_a:\n{tensor_a}")
tensor_b = torch.tensor([[2, 3, 4], [5, 6, 7]])
print(f"tensor_b:\n{tensor_b}")
print(f"result:\n{tensor_a * tensor_b}")
```

- 运行结果

```
normal calculator
tensor_a:
tensor([[1, 2, 3],
        [4, 5, 6]])
tensor_b:
tensor([[2, 3, 4],
        [5, 6, 7]])
result:
tensor([[ 2,  6, 12],
        [20, 30, 42]])
```


## 2. 一个张量与另一个张量的后半部分维度相同

- 这种情况是`形状不同的维度，至少有一方的维度为1`的一种特殊情况，它的所有1维度，都是由维度较低的张量提供的
- 比如：`tensor(2,3,2)`与`tensor(3,2)`,`tensor(3,2)`是`tensor(2,3,2)`的后半部分
- 实际上，可以被写成`tensor(2,3,2)`与`tensor(1,3,2)`，实际上是由`tensor(1,3,2)`提供了第一个维度不同时的`一方维度为1`的条件

```python
import torch

print("front broadcast")
tensor_a = torch.tensor(
    [[[1, 2], [3, 4], [5, 6]], [[1, 2], [3, 4], [5, 6]]]
)
print(f"tensor_a shape:\n{tensor_a.shape}")

tensor_b = torch.tensor(
    [[2, 3], [4, 5], [6, 7]]
)
print(f"tensor_b shape:\n{tensor_b.shape}")

result = tensor_a * tensor_b
print(f"result shape:\n{result.shape}")
print(f"result:\n{result}")
```

- 运行结果
```
front broadcast
tensor_a shape:
torch.Size([2, 3, 2])
tensor_b shape:
torch.Size([3, 2])
result shape:
torch.Size([2, 3, 2])
result:
tensor([[[ 2,  6],
         [12, 20],
         [30, 42]],

        [[ 2,  6],
         [12, 20],
         [30, 42]]])
```

- 可以看到，操作符是将所有的后向元素，逐一匹配到前向元素上，match成为一个前向元素的子集，进行操作的
- 这样可以在一定程度上替代循环操作，防止大量的跨语言交互

## 3. 推广到一般情况

- 这时，我们考虑推广到一般情况，即`形状不同的维度，至少有一方的维度为1`的全部情况
- 假设这个形状不同的维度，出现在中间，而不是前侧，将会出现什么情况？

```python
import torch

print("normal broadcast")
tensor_a = torch.tensor(
    [[[1, 2], [3, 4], [5, 6]], [[3, 4], [5, 6], [7, 8]]]
)
print(f"tensor_a shape:\n{tensor_a.shape}")

tensor_b = torch.tensor(
    [[[2, 3]], [[4, 5]]]
)
print(f"tensor_b shape:\n{tensor_b.shape}")

result = tensor_a * tensor_b
print(f"result shape:\n{result.shape}")
print(f"result:\n{result}")
```

- 运行结果
```
normal broadcast
tensor_a shape:
torch.Size([2, 3, 2])
tensor_b shape:
torch.Size([2, 1, 2])
result shape:
torch.Size([2, 3, 2])
result:
tensor([[[ 2,  6],
         [ 6, 12],
         [10, 18]],

        [[12, 20],
         [20, 30],
         [28, 40]]])
```

- 可以看到，广播机制复制了`[[[2, 3]], [[4, 5]]]`，等效于`[[[2, 3], [2, 3], [2, 3]], [[4, 5], [4, 5], [4, 5]]]`
- 把维度比做一颗树的话，那么就是进行了一次深度优先的***后序遍历***，扩散了当前节点的值