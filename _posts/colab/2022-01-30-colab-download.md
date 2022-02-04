---
layout: single
title:  "Colab 下载文件"
date:   2022-01-30 20:22:51 +0800
categories:
  - colab
tags:
  - 下载
---

# 使用 google.colab包
```python
from google.colab import files
files.download(path/'labels.csv')
```