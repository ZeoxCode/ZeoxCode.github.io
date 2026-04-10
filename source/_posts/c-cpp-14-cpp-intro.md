---
title: C/C++学习总结（十四）——C++入门
date: 2016-10-17
categories:
  - 全栈开发
  - 后端
tags:
  - C++
---

## 与C的主要区别

```cpp
// 输入输出
#include <iostream>
using namespace std;

cout << "hello" << endl;
cin >> x;
```

替代 `printf` / `scanf`，不需要格式符。

## 引用

```cpp
int a = 10;
int &ref = a;  // ref是a的别名
ref = 20;
cout << a;  // 20
```

引用必须在声明时初始化，且不能改变引用对象。

与指针的区别：引用不能为NULL，不能重新指向其他变量。

## bool 类型

```cpp
bool flag = true;
bool ok = false;
```

C语言没有bool（C99有_Bool，但不常用），C++原生支持。

## string 类

```cpp
#include <string>
string s = "hello";
s += " world";
cout << s.length() << endl;
cout << (s == "hello world") << endl;  // 可以直接用==比较
```

---

**问题：** 混用 `cin` 和 `scanf` 导致输入异常
**解决：** 同一程序统一用一套 I/O，不要混用。若必须混用：
```cpp
ios::sync_with_stdio(false);
```

---

**问题：** `using namespace std` 导致命名冲突
```cpp
using namespace std;
int count = 0;  // 与std::count冲突
```
**解决：** 改用 `std::cout`、`std::cin` 显式指定，或避开冲突名称。
