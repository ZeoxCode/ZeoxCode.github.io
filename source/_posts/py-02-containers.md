---
title: Python 02 - 装数据的四种盒子
date: 2017-08-21
tags: [Python]
categories:
  - 编程语言
  - Python
---

## List 列表

有序、可修改，最常用。

```python
nums = [1, 2, 3, 4, 5]

nums[0]           # 1（正索引）
nums[-1]          # 5（负索引）
nums[1:3]         # [2, 3]（切片，不含右端）
nums[::-1]        # [5, 4, 3, 2, 1]（反转）

nums.append(6)    # 末尾加一个
nums.extend([7, 8])  # 末尾加一批
nums.insert(0, 0) # 指定位置插入
nums.pop()        # 删除末尾，返回该元素
nums.pop(0)       # 删除指定位置
nums.remove(3)    # 删除第一个值为 3 的元素
nums.sort()       # 原地排序
nums.sort(reverse=True)  # 降序
sorted(nums)      # 返回新列表，不修改原列表
len(nums)         # 长度
3 in nums         # True（判断是否包含）
```

## Tuple 元组

有序、**不可修改**，适合存不该变的数据。

```python
point = (3, 4)
x, y = point      # 解包

# 单元素元组必须加逗号
t = (1,)   # 这是元组
t = (1)    # 这是 int，不是元组！

# 元组比列表快，常用于函数返回多个值
def min_max(lst):
    return min(lst), max(lst)

lo, hi = min_max([3, 1, 4, 1, 5])
```

## Dict 字典

键值对，无序（Python 3.7+ 保持插入顺序）。

```python
person = {"name": "Alice", "age": 20}

person["name"]          # "Alice"
person.get("age")       # 20
person.get("email", "") # 不存在时返回默认值，不报错

person["age"] = 21      # 修改
person["email"] = "alice@example.com"  # 新增
del person["email"]     # 删除

"name" in person        # True（判断 key 是否存在）

person.keys()           # dict_keys(["name", "age"])
person.values()         # dict_values(["Alice", 21])
person.items()          # dict_items([("name", "Alice"), ("age", 21)])

# 遍历
for k, v in person.items():
    print(k, "->", v)
```

## Set 集合

无序、**不重复**，常用于去重和集合运算。

```python
s = {1, 2, 3, 3, 2}   # {1, 2, 3}，自动去重

s.add(4)
s.remove(1)

a = {1, 2, 3}
b = {2, 3, 4}
a | b   # {1, 2, 3, 4}（并集）
a & b   # {2, 3}（交集）
a - b   # {1}（差集）
```

## 常见坑

- 空字典是 `{}`，空集合必须用 `set()`，不能用 `{}`（那是字典）
- `dict.get(key)` 不存在时返回 `None`，不报错；`dict[key]` 不存在直接抛 `KeyError`
- 列表可以作为集合或字典的 key 吗？不行，列表不可哈希（unhashable）
- 元组切片返回元组，列表切片返回列表
