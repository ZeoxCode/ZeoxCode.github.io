---
title: Python 01 - 第一行 Python
date: 2017-08-14
tags: [Python]
categories:
  - 编程语言
  - Python
---

## 跑起来

```python
print("Hello, World!")
```

```bash
python3 hello.py
```

不用编译，直接运行。没有 `main` 函数，没有分号，没有大括号。

## 变量

```python
x = 10
name = "Alice"
pi = 3.14
flag = True

# 多重赋值
a, b, c = 1, 2, 3

# 交换两个变量，不需要临时变量
a, b = b, a
```

## 数据类型

```python
type(42)        # <class 'int'>
type(3.14)      # <class 'float'>
type("hello")   # <class 'str'>
type(True)      # <class 'bool'>
type(None)      # <class 'NoneType'>
```

## 数字运算

```python
10 + 3    # 13
10 - 3    # 7
10 * 3    # 30
10 / 3    # 3.3333...（结果是 float）
10 // 3   # 3（整除）
10 % 3    # 1（取余）
2 ** 10   # 1024（幂运算，不是 ^）
```

## 字符串

```python
s = "Hello, World!"

len(s)               # 13
s.upper()            # "HELLO, WORLD!"
s.lower()            # "hello, world!"
s.strip()            # 去首尾空白
s.replace("l", "r")  # "Herro, Worrd!"
s.split(", ")        # ["Hello", "World!"]
s[0]                 # "H"
s[1:5]               # "ello"
s[-1]                # "!"（负索引从末尾算）

# 格式化
name = "Alice"
age = 20
print("My name is %s, I am %d years old." % (name, age))   # 旧风格
print("My name is {}, I am {} years old.".format(name, age)) # 新风格
```

## 输入

```python
name = input("请输入你的名字：")   # 返回的永远是字符串
age  = int(input("请输入年龄："))  # 需要手动转换
```

## 常见坑

- `/` 永远是真除法，`5 / 2 = 2.5`；整除用 `//`
- `input()` 返回字符串，做数学运算前必须转换
- Python 用**缩进**表示代码块，不是大括号，混用空格和 Tab 会报错
- `**` 是幂运算，`^` 是按位异或，别搞混
