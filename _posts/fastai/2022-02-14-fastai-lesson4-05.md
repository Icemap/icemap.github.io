---
title:  "fast.ai 学习 第四课笔记 - 品种分类"
date:   2022-02-14 17:42:00 +0800
categories:
  - fast.ai
tags:
  - 笔记
---

# 1. Tensor索引

- 开了眼儿了

```python
from fastai.vision.all import *
src_data = tensor([[1, 2, 3], [4, 5, 6]])
print(src_data[tensor([1, 0, 0]), tensor([2, 1, 2])])
```

结果是：

```
tensor([6, 2, 3])
```

- 就是说，可以传数个Tensor或者数组作为Index，批量取出数据
- 真的是那个讲师说的`super handy`啊