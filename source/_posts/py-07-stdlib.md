---
title: Python 07 - 自带的瑞士军刀
date: 2017-09-25
tags: [Python]
categories:
  - 编程语言
  - Python
---

## os — 操作系统接口

```python
import os

os.getcwd()                    # 当前工作目录
os.listdir(".")                # 列出目录下的文件
os.path.exists("file.txt")     # 文件/目录是否存在
os.path.join("dir", "file.txt") # 拼接路径（跨平台安全）
os.path.basename("/a/b/c.txt") # "c.txt"
os.path.dirname("/a/b/c.txt")  # "/a/b"
os.path.splitext("file.txt")   # ("file", ".txt")

os.makedirs("a/b/c", exist_ok=True)  # 递归创建目录
os.rename("old.txt", "new.txt")
os.remove("file.txt")

# 遍历目录树
for root, dirs, files in os.walk("./src"):
    for f in files:
        print(os.path.join(root, f))
```

## sys — 解释器相关

```python
import sys

sys.argv        # 命令行参数列表，argv[0] 是脚本名
sys.exit(0)     # 退出程序
sys.version     # Python 版本字符串
sys.path        # 模块搜索路径
sys.platform    # 'win32' / 'linux' / 'darwin'
```

## datetime — 时间日期

```python
from datetime import datetime, timedelta

now = datetime.now()
print(now)                        # 2017-09-25 10:30:00.123456

# 格式化
now.strftime("%Y-%m-%d %H:%M:%S") # "2017-09-25 10:30:00"

# 解析字符串
dt = datetime.strptime("2017-09-25", "%Y-%m-%d")

# 时间运算
tomorrow = now + timedelta(days=1)
diff = datetime(2018, 1, 1) - now
print(diff.days)   # 距离元旦还有多少天
```

## re — 正则表达式

```python
import re

text = "手机号：138-0013-8000，备用：13900001234"

# 查找一个
m = re.search(r"\d{3}-\d{4}-\d{4}", text)
if m:
    print(m.group())   # 138-0013-8000

# 查找所有
phones = re.findall(r"1[3-9]\d{9}", text)

# 替换
result = re.sub(r"\d{4}$", "****", "13812345678")  # "1381234****"

# 分割
parts = re.split(r"[,，、]", "苹果,香蕉，橙子、葡萄")
```

## collections — 好用的容器扩展

```python
from collections import Counter, defaultdict, OrderedDict, deque

# Counter：统计频次
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
c = Counter(words)
print(c)                   # Counter({'apple': 3, 'banana': 2, 'cherry': 1})
print(c.most_common(2))    # [('apple', 3), ('banana', 2)]

# defaultdict：访问不存在的 key 时自动创建默认值
graph = defaultdict(list)
graph["A"].append("B")     # 不用先判断 "A" 是否存在

# deque：双端队列，两端操作都是 O(1)
q = deque([1, 2, 3])
q.appendleft(0)   # [0, 1, 2, 3]
q.popleft()       # 0
```

## random

```python
import random

random.random()           # [0.0, 1.0) 的浮点数
random.randint(1, 100)    # 1~100 的整数（含两端）
random.choice([1, 2, 3])  # 随机取一个
random.shuffle(lst)       # 原地打乱列表
random.sample(lst, k=3)   # 随机取 k 个，不重复
```

## 常见坑

- `os.path.join` 在 Windows 返回反斜杠路径，在 Linux 返回正斜杠，不要手动拼字符串
- `re.match` 只匹配字符串开头，`re.search` 匹配任意位置，别搞混
- `datetime.now()` 返回的是本地时间，不带时区信息
