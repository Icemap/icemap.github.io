---
title:  "fast.ai 学习 第三课笔记"
date:   2022-01-30 20:01:00 +0800
categories:
  - fast.ai
tags:
  - 笔记
---

# 1. 写Blog

- 这个老师让我们写Blog，这也是我为什么开始写Blog的原因

# 2. L1范数/L2范数

- 范数，就是一个求"长度"的函数
- 例如，在空间中，有一个二维向量(3,4)，那么它的长度：
- - 使用L1范数求得，就是3+4=7
- - 使用L2范数求得，就是sqrt(3^2 + 4^2) = 5
- 所以L2范数也叫欧式范数

# 3. L1/L2 范数推导，F.l1_loss/F.mse_loss使用
```python
from fastai.vision.all import *
matplotlib.rc('image', cmap='Greys')

# 加载MNIST的一个子集，仅包含3/7两个数字的分辨
path = untar_data(URLs.MNIST_SAMPLE)

# 把数据集内的所有路径拉取出来，变为数组
threes = (path/'train'/'3').ls().sorted()
sevens = (path/'train'/'7').ls().sorted()

# 数组解析表达式，把python的路径数组转为PyTorch的图片的二维张量的数组
three_tensors = [tensor(Image.open(path)) for path in threes]
seven_tensors = [tensor(Image.open(path)) for path in sevens]

# 将二维张量的数组，变为三维张量
stacked_three_tensors = torch.stack(three_tensors).float() / 255
stacked_seven_tensors = torch.stack(seven_tensors).float() / 255

# 打印一下三维张量的Shape
print(f"tensor three shape: {stacked_three_tensors.shape}")
print(f"tensor seven shape: {stacked_seven_tensors.shape}")

# 沿着0号坐标轴(axis=0)的方向，取平均值，将三维张量压回二维
three_mean = stacked_three_tensors.mean(axis=0)
seven_mean = stacked_seven_tensors.mean(axis=0)

# 打印一下平均后的压平的二维张量
print(f"mean three shape: {three_mean.shape}")
print(f"mean seven shape: {seven_mean.shape}")

# 取出随便一个图片
a_3 = stacked_three_tensors[1]

# 比对与3/7的相对距离，评价方式有两种：
# L1范数/L2范数
#    L1 范数就是：
#      1. 将所有的值进行差值
#      2. 对差值进行求绝对值
#      3. 对结果进行求平均值
#    L2 范数就是：
#      1. 将所有的值进行差值
#      2. 对差值进行平方(^2)
#      3. 对结果进行求平均值
#      4. 对结果进行开方(^0.5)

# L1范数计算
l1_3_delta = (a_3 - three_mean).abs().mean()
l1_7_delta = (a_3 - seven_mean).abs().mean()

# L2范数计算
l2_3_delta = ((a_3 - three_mean) ** 2).abs().mean().sqrt()
l2_7_delta = ((a_3 - seven_mean) ** 2).abs().mean().sqrt()

# 打印L1，L2范数
print(f"l1 three: {l1_3_delta}, seven: {l1_7_delta}")
print(f"l2 three: {l2_3_delta}, seven: {l2_7_delta}")

# 直接使用内置函数：l1_loss/mse_loss，代替我们自己写的 L1/L2范数计算过程
# 注意mse_loss，是返回不开方的结果的，需要手动开方
print("inner function:")
print(f"l1 three: {F.l1_loss(a_3.float(), three_mean.float())}, "
      f"seven: {F.l1_loss(a_3.float(), three_mean.float())}")
print(f"l2 three: {F.mse_loss(a_3.float(), three_mean.float()).sqrt()}, "
      f"seven: {F.mse_loss(a_3.float(), three_mean.float()).sqrt()}")

```
