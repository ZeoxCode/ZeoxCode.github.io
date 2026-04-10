---
title: C/C++学习总结（四）——流程控制
date: 2016-09-15
categories:
  - 全栈开发
  - 后端
tags:
  - C
---

## if / else if / else

```c
if (条件1) {
    ...
} else if (条件2) {
    ...
} else {
    ...
}
```

C中 0 为 false，非0为 true。

## switch

```c
switch (变量) {
    case 值1:
        ...
        break;
    case 值2:
        ...
        break;
    default:
        ...
}
```

注：switch 只支持整型和字符，不支持 float 和字符串。

## 三目运算符

```c
int max = (a > b) ? a : b;
```

---

**问题：** switch 中漏写 `break` 导致 case 穿透
```c
switch (x) {
    case 1:
        printf("one\n");
        // 漏了break
    case 2:
        printf("two\n");  // x=1时也会执行这行
        break;
}
```
**解决：** 每个 case 末尾加 `break`，除非有意穿透。
