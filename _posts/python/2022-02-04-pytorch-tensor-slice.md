---
layout: single
title:  "pytorch 张量切片"
date:   2022-02-04 15:47:51 +0800
categories:
  - python grammer
tags:
	- 语法
	- pytorch
---

# 张量切片

- 类比数组切片，暂时还没发现有什么区别

```python
from fastai.vision.all import *

# 随机出来一个二维张量
tensor_object = torch.randn(4, 3)

# 打印一下
print(tensor_object)

# 取第二行
print(tensor_object[1])

# 取第一列
print(tensor_object[:, 0])

# 取第二列的 [1, 3) 元素，切片为左闭右开区间
print(tensor_object[1:3, 1])

# 取第三行的 [倒数第三个, 倒数第一个)元素
print(tensor_object[2, -3:-1])

# 当然，你也可以是一个原教旨主义者，这个等效于上面的一行, len(tensor_object) - 1为最后一个index
print(tensor_object[2, len(tensor_object) - 1 - 3:len(tensor_object) - 1 - 1])

```