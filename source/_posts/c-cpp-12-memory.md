---
title: C/C++学习总结（十二）——内存管理
date: 2016-10-11
categories:
  - 全栈开发
  - 后端
tags:
  - C
---

## 栈与堆

- **栈**：局部变量，函数返回自动释放
- **堆**：动态分配，手动 malloc / free

## malloc / calloc / free

```c
#include <stdlib.h>

int *p = (int*)malloc(10 * sizeof(int));  // 分配，不初始化
int *p = (int*)calloc(10, sizeof(int));   // 分配，初始化为0

free(p);
p = NULL;
```

---

**问题：** malloc 返回 NULL 未判断，直接使用导致段错误
```c
int *p = (int*)malloc(1024 * 1024 * 1024);
*p = 1;  // 如果分配失败p是NULL，段错误
```
**解决：**
```c
if (p == NULL) {
    fprintf(stderr, "malloc failed\n");
    return -1;
}
```

---

**问题：** 忘记 free，内存泄漏
```c
void func() {
    int *p = (int*)malloc(100 * sizeof(int));
    // 没有free，函数返回后p丢失，内存无法回收
}
```
**解决：** 每个 malloc 对应一个 free。

---

**问题：** double free，程序崩溃
```c
free(p);
free(p);  // 第二次free，未定义行为
```
**解决：** free 后立即 `p = NULL`，对 NULL free 是安全的。

参考：http://blog.csdn.net/daiyutage/article/details/8483554
