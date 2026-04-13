---
title: Python 03 - 流程控制与推导式
date: 2017-08-28
tags: [Python]
categories:
  - 编程语言
  - Python
---

## if / elif / else

```python
score = 85

if score >= 90:
    print("优秀")
elif score >= 60:
    print("及格")
else:
    print("不及格")

# 一行写法（三元表达式）
result = "及格" if score >= 60 else "不及格"
```

## for 循环

```python
# 遍历列表
for x in [1, 2, 3]:
    print(x)

# range
for i in range(5):       # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):    # 1, 2, 3, 4, 5
    print(i)

for i in range(0, 10, 2):  # 0, 2, 4, 6, 8（步长）
    print(i)

# 同时拿索引和值
fruits = ["apple", "banana", "cherry"]
for i, fruit in enumerate(fruits):
    print(i, fruit)

# 同时遍历两个列表
for a, b in zip([1, 2, 3], ["x", "y", "z"]):
    print(a, b)
```

## while 循环

```python
n = 1
while n <= 5:
    print(n)
    n += 1

# break / continue
for i in range(10):
    if i == 3:
        continue    # 跳过本次
    if i == 7:
        break       # 跳出循环
    print(i)
```

## 列表推导式

Python 最有特色的写法之一。

```python
# 基本形式
squares = [x ** 2 for x in range(10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# 带过滤条件
evens = [x for x in range(20) if x % 2 == 0]
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# 嵌套
pairs = [(x, y) for x in [1, 2] for y in [3, 4]]
# [(1, 3), (1, 4), (2, 3), (2, 4)]
```

字典和集合也有推导式：

```python
# 字典推导式
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# 集合推导式
unique = {x % 3 for x in range(10)}
# {0, 1, 2}
```

## 常见坑

- Python 没有 `switch`，多分支用 `if / elif`
- `for` 遍历时不要在循环体内修改正在遍历的列表，会出 bug；先复制一份再改
- `range(10)` 不是列表，是一个惰性对象；要转成列表用 `list(range(10))`
- 推导式嵌套超过两层就别用了，可读性太差，老老实实写 for
