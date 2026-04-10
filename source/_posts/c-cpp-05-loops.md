---
title: C/C++学习总结（五）——循环
date: 2016-09-18
categories:
  - 全栈开发
  - 后端
tags:
  - C
---

## for

```c
for (int i = 0; i < n; i++) {
    ...
}
```

注：C89 不允许在 for 内声明变量，需在外部声明。C99 起支持。

## while

```c
while (条件) {
    ...
}
```

## do-while

```c
do {
    ...
} while (条件);
```

区别：do-while 至少执行一次。

## break / continue

- `break`：退出整个循环
- `continue`：跳过本次，继续下次

---

**问题：** 死循环——循环变量未更新
```c
int i = 0;
while (i < 10) {
    printf("%d\n", i);
    // 忘了 i++
}
```
**解决：** 检查循环变量是否在循环体内有更新。

---

**问题：** 数组遍历越界（off-by-one）
```c
int arr[10];
for (int i = 0; i <= 10; i++) {  // i <= 10 错误，arr[10]越界
    arr[i] = i;
}
```
**解决：** 统一用 `i < n`，不用 `i <= n-1`
```c
for (int i = 0; i < 10; i++) { ... }
```
