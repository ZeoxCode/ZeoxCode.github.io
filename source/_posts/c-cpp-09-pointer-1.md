---
title: C/C++学习总结（九）——指针（上）
date: 2016-10-01
categories:
  - 全栈开发
  - 后端
tags:
  - C
---

## 基本概念

指针：存储内存地址的变量。

```c
int a = 10;
int *p = &a;   // p 存储 a 的地址

printf("%p\n", p);   // 输出地址
printf("%d\n", *p);  // 解引用，输出10
```

- `&`：取地址
- `*`：解引用（取该地址的值）

## 通过指针修改变量

```c
int a = 10;
int *p = &a;
*p = 20;
printf("%d\n", a);  // 20
```

## 指针类型

不同类型指针解引用读取的字节数不同，类型须匹配：

```c
int a = 10;
double *p = &a;  // 错误，类型不匹配
```

## NULL 指针

```c
int *p = NULL;  // 暂不指向任何地址时初始化为NULL
if (p != NULL) {
    *p = 10;
}
```

---

**问题：** 对未初始化的指针解引用，程序崩溃（Segmentation fault）
```c
int *p;
*p = 10;  // p是随机地址，段错误
```
**解决：** 声明指针后立即初始化为 NULL，使用前判断
```c
int *p = NULL;
```
参考：http://blog.csdn.net/u012819339/article/details/50880765
