---
title: Python 06 - 出错了别慌，还有文件要读
date: 2017-09-18
tags: [Python]
categories:
  - 编程语言
  - Python
---

## 异常处理

```python
try:
    x = int(input("输入一个数字："))
    result = 10 / x
    print(result)
except ValueError:
    print("输入的不是数字")
except ZeroDivisionError:
    print("不能除以零")
except Exception as e:
    print(f"出了点问题：{e}")
else:
    print("没有异常，正常执行完了")    # try 块没有异常时执行
finally:
    print("不管有没有异常都会执行")    # 一般用来释放资源
```

## 常见内置异常

```python
int("abc")           # ValueError
1 / 0                # ZeroDivisionError
[][10]               # IndexError
{}["key"]            # KeyError
None.strip()         # AttributeError
open("不存在.txt")   # FileNotFoundError
```

## 主动抛出异常

```python
def set_age(age):
    if age < 0 or age > 150:
        raise ValueError(f"年龄不合法：{age}")
    return age
```

## 文件读写

```python
# 读文件（推荐用 with，自动关闭）
with open("data.txt", "r", encoding="utf-8") as f:
    content = f.read()       # 一次读全部
    # 或
    lines = f.readlines()    # 按行读成列表
    # 或
    for line in f:           # 逐行迭代（节省内存）
        print(line.strip())

# 写文件
with open("output.txt", "w", encoding="utf-8") as f:
    f.write("Hello\n")
    f.writelines(["line1\n", "line2\n"])

# 追加
with open("log.txt", "a", encoding="utf-8") as f:
    f.write("新的一行\n")
```

## 文件模式

| 模式 | 说明 |
|------|------|
| `r`  | 只读（默认），文件不存在报错 |
| `w`  | 只写，文件不存在则创建，存在则清空 |
| `a`  | 追加，不清空原内容 |
| `rb` / `wb` | 二进制模式（读图片、视频等） |

## JSON 读写

```python
import json

# 写 JSON
data = {"name": "Alice", "scores": [90, 85, 92]}
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

# 读 JSON
with open("data.json", "r", encoding="utf-8") as f:
    data = json.load(f)

# 字符串和字典互转
s = json.dumps(data)           # dict → str
d = json.loads('{"a": 1}')    # str → dict
```

## 常见坑

- Windows 上默认编码不是 UTF-8，读中文文件一定要加 `encoding="utf-8"`
- `f.read()` 之后再调一次 `f.read()` 会返回空字符串，因为文件指针已经到末尾了
- 用 `with` 语句，不要手动 `f.close()`，出异常时也能保证文件关闭
- `w` 模式会清空文件，想保留原内容用 `a` 模式
