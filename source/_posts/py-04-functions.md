---
title: Python 04 - 函数，把代码打包带走
date: 2017-09-04
tags: [Python]
categories:
  - 编程语言
  - Python
---

## 基本定义

```python
def greet(name):
    return "Hello, " + name

print(greet("Alice"))   # Hello, Alice
```

## 默认参数

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Alice")            # "Hello, Alice!"
greet("Alice", "Hi")      # "Hi, Alice!"
```

## 关键字参数

```python
def person_info(name, age, city):
    print(f"{name}, {age}岁, 来自{city}")

person_info("Alice", city="北京", age=20)  # 顺序可以乱
```

## 可变参数

```python
# *args：接收任意个位置参数，打包成元组
def add(*args):
    return sum(args)

add(1, 2, 3)    # 6
add(1, 2, 3, 4) # 10

# **kwargs：接收任意个关键字参数，打包成字典
def show(**kwargs):
    for k, v in kwargs.items():
        print(k, "=", v)

show(name="Alice", age=20)
```

## 返回多个值

```python
def min_max(lst):
    return min(lst), max(lst)   # 实际返回的是元组

lo, hi = min_max([3, 1, 4, 1, 5])
```

## lambda

适合写简单的一次性函数。

```python
square = lambda x: x ** 2
square(5)   # 25

# 常和 sorted、map、filter 一起用
nums = [3, 1, 4, 1, 5, 9]
sorted(nums, key=lambda x: -x)  # 降序排列

pairs = [(1, "b"), (3, "a"), (2, "c")]
sorted(pairs, key=lambda p: p[1])  # 按第二个元素排
```

## 作用域

```python
x = 10   # 全局变量

def foo():
    x = 20   # 局部变量，不影响全局的 x
    print(x) # 20

foo()
print(x)     # 10

# 要修改全局变量，需要声明 global
def bar():
    global x
    x = 99

bar()
print(x)     # 99
```

## 常见坑

- 默认参数如果是可变对象（如列表），多次调用会共享同一个对象：

```python
# 错误写法
def append_to(item, lst=[]):
    lst.append(item)
    return lst

append_to(1)  # [1]
append_to(2)  # [1, 2]  ← 不是 [2]！

# 正确写法
def append_to(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

- `lambda` 只能写表达式，不能写 `if/for` 等语句
- Python 函数没有返回值时默认返回 `None`
