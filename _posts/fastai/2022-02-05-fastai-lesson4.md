---
title:  "fast.ai 学习 第四课笔记"
date:   2022-01-30 20:01:00 +0800
categories:
  - fast.ai
tags:
  - 笔记
---

# 1. 解构元组

- 把元组赋值到多个对象中

```python
def some_function(tuple_item) -> (int, int):
    return tuple_item[0], tuple_item[1]

a = (1, 2)
a1, a2 = some_function(a)
print(a1, a2)
```

- 运行结果
```
1 2
```

# 2. Tensor.view() 函数

- 作用为reshape
- view的-1参数使用方案探索如下：

```python
from fastai.vision.all import *

tensor_object = torch.randn(4, 3)

print(f"view(-1): {tensor_object.view(-1)}")
print(f"view(4, -1): {tensor_object.view(4, -1)}")
print(f"view(-1, 3): {tensor_object.view(-1, 3)}")
print(f"view(-1, -1, 3): {tensor_object.view(-1, -1, 3)}")
print(f"view(-1, 5): {tensor_object.view(-1, 5)}")
```

- 运行结果

```
view(-1): tensor([ 0.7477,  0.2016, -0.4106, -1.5348, -0.4857, -0.8893,  2.1312,  0.8525,
         0.4748,  0.0269,  0.0769, -0.1883])
view(4, -1): tensor([[ 0.7477,  0.2016, -0.4106],
        [-1.5348, -0.4857, -0.8893],
        [ 2.1312,  0.8525,  0.4748],
        [ 0.0269,  0.0769, -0.1883]])
view(-1, 3): tensor([[ 0.7477,  0.2016, -0.4106],
        [-1.5348, -0.4857, -0.8893],
        [ 2.1312,  0.8525,  0.4748],
        [ 0.0269,  0.0769, -0.1883]])
Traceback (most recent call last):
  File "/Users/xxxxxxxx.py", line x, in <module>
    print(f"view(-1, -1, 3): {tensor_object.view(-1, -1, 3)}")
RuntimeError: only one dimension can be inferred
Traceback (most recent call last):
  File "/Users/xxxxxxxx.py", line x, in <module>
    print(f"view(-1, 5): {tensor_object.view(-1, 5)}")
RuntimeError: shape '[-1, 5]' is invalid for input of size 12
```

- 可以看出以下几点：
- - 仅有-1作为参数时，将reshape tensor到一维
- - 除了-1，还有其他参数时，将参考其他参数与tensor大小，自动确定维度
- - 不可有多个-1（未指定维度）
- - 不可指定不能整除的组合

# 3. Tensor和list在面对加减乘除时有不同返回

- 其中，list和list不支持减法，list和int不支持除法，也就是说，原生的Python是不支持“广播”机制的
- 而且，在乘法和加法中，list和Tensor的表现也是不一致的
- 加法中：
- - Tensor：对位元素逐一求和
- - list：连接两list
- 乘法中：
- - Tensor：对每个元素都乘以该数
- - list：复制当前list该数倍

```python
print(torch.tensor([1, 1]) * 5)
print([1, 1] * 5)

print(torch.tensor([1, 1]) + torch.tensor([1, 1]))
print([1, 1] + [1, 1])

print(torch.tensor([1, 1]) - torch.tensor([1, 1]))
# print([1, 1] - [1, 1])
'''
Traceback (most recent call last):
  File "/Users/xxxxxx.py", line 20, in <module>
    print([1, 1] - [1, 1])
TypeError: unsupported operand type(s) for -: 'list' and 'list'
'''

print(torch.tensor([1, 1]) / 1)
# print([1, 1] / 1)
'''
Traceback (most recent call last):
  File "/Users/xxxxxx.py", line 29, in <module>
    print([1, 1] / 1)
TypeError: unsupported operand type(s) for /: 'list' and 'int'
'''
```
