---
layout: single
title:  "Python推导式"
date:   2022-02-04 15:47:51 +0800
categories:
  - python grammer
tags:
  - 语法
---

## 1. 列表推导式
```python
# list
print("list")

list_a = [item * item for item in range(10)]
print(list_a)

list_to_string = [str(i) for i in range(10)]
print(list_to_string)

list_condition = [i for i in range(10) if i > 3]
print(list_condition)

list_condition_else = [i * i if i > 5 else str(i) for i in range(10)]
print(list_condition_else)
```

## 2. 元组推导式

- 这玩意出来是个生成器，因为元组不可变，所以只能这么返回，符合语法
- 生成器读取一遍，数据就没了

```python
# tuple
print("\ntuple")
tuple_a_generator = (item * item for item in range(3, 5))
print(tuple_a_generator)
print(tuple(tuple_a_generator))
print(tuple(tuple_a_generator))
```

## 3. 映射推导式
```python
# map
print("\nmap")
map_a = {item["key"]: item for item in [{'key': 1, 'name': 'a'}, {'key': 2, 'name': 'b'}, {'key': 3, 'name': 'c'}]}
print(map_a)
```