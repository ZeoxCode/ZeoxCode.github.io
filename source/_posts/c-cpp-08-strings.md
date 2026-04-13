---
title: C/C++学习总结（八）——字符串
date: 2016-09-27
categories:
  - 编程语言
  - C/C++
tags:
  - C
---

## 基本概念

C 字符串 = 字符数组 + `\0` 结尾

```c
char str[10] = "hello";
// 存储：'h' 'e' 'l' 'l' 'o' '\0' ...
```

声明长度需包含 `\0`，"hello" 需要长度至少 6。

## 常用函数（需 `#include <string.h>`）

```c
strlen(str)           // 返回长度（不含\0）
strcpy(dst, src)      // 复制
strcat(dst, src)      // 拼接
strcmp(s1, s2)        // 比较，相等返回0
strncpy(dst, src, n)  // 最多复制n个字符
```

## 输入

```c
scanf("%s", str);      // 读到空格停止
fgets(str, n, stdin);  // 读整行，最多n-1字符
```

---

**问题：** 用 `==` 比较字符串
```c
if (s1 == s2) { ... }  // 比较的是地址，不是内容
```
**解决：**
```c
if (strcmp(s1, s2) == 0) { ... }
```

---

**问题：** `gets()` 缓冲区溢出，输入超长导致越界
```c
char str[10];
gets(str);  // 不检查长度，unsafe
```
**解决：** 用 `fgets` 替代
```c
fgets(str, sizeof(str), stdin);
// 注意：fgets会把'\n'也读进来，需要处理
str[strcspn(str, "\n")] = '\0';
```
参考：http://blog.csdn.net/zhengqijun_/article/details/52782202

---

**问题：** `strncpy` 不保证末尾有 `\0`
```c
strncpy(dst, src, n);
dst[n] = '\0';  // 需手动补\0
```
