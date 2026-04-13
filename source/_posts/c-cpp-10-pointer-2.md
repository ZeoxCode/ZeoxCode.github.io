---
title: C/C++学习总结（十）——指针（下）
date: 2016-10-05
categories:
  - 编程语言
  - C/C++
tags:
  - C
---

## 指针与数组

数组名是指向首元素的指针：

```c
int arr[5] = {10, 20, 30, 40, 50};
int *p = arr;

printf("%d\n", *(p+1));  // 20
printf("%d\n", *(p+2));  // 30
```

等价关系：`arr[i]` ↔ `*(arr + i)`

`p+1` 移动的是一个元素的大小，不是1个字节。

## 指针作为函数参数

```c
void swap(int *a, int *b) {
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

int x = 3, y = 5;
swap(&x, &y);
printf("%d %d\n", x, y);  // 5 3
```

## 二级指针

```c
int a = 10;
int *p = &a;
int **pp = &p;

printf("%d\n", **pp);  // 10
```

---

**问题：** 悬空指针（dangling pointer）——free 后继续使用
```c
int *p = (int*)malloc(sizeof(int));
free(p);
*p = 20;  // 未定义行为，可能崩溃
```
**解决：** free 后立即置 NULL
```c
free(p);
p = NULL;
```
参考：http://blog.csdn.net/hackbuteer1/article/details/6712003

---

**问题：** 函数内修改指针本身不影响外部（指针也是值传递）
```c
void func(int *p) {
    p = NULL;  // 只改了局部的p，外部指针不变
}
```
**解决：** 要修改指针本身，传二级指针 `int **p`。
