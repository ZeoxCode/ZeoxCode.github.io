---
title: C/C++学习总结（六）——函数与递归
date: 2016-09-21
categories:
  - 全栈开发
  - 后端
tags:
  - C
---

## 基本结构

```c
返回类型 函数名(参数列表) {
    ...
    return 返回值;
}
```

## 函数声明

定义在调用之后时，需先声明：

```c
int add(int a, int b);  // 声明

int main() {
    add(3, 4);
    return 0;
}

int add(int a, int b) { return a + b; }  // 定义
```

## 值传递

```c
void change(int x) { x = 100; }  // 只改局部变量，外部不受影响

int a = 5;
change(a);
printf("%d\n", a);  // 仍为 5
```

要修改外部变量须传指针。

## 递归

```c
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

---

**问题：** 递归无终止条件导致栈溢出（程序崩溃）
```c
int f(int n) {
    return n + f(n - 1);  // 没有出口
}
```
**解决：** 每个递归函数必须有明确的终止条件（base case）。

---

**问题：** 函数未声明直接调用，编译报错
**解决：** 1. 把函数定义移到 main 之前；2. 或在 main 前加函数声明。
