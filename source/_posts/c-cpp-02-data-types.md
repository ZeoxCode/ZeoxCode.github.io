---
title: C/C++学习总结（二）——基本数据类型
date: 2016-09-09
categories:
  - 编程语言
  - C/C++
tags:
  - C
---

## 常用类型

| 类型 | 大小 | 说明 |
|------|------|------|
| int | 4字节 | 整数 |
| short | 2字节 | 短整数 |
| long | 4/8字节 | 平台相关 |
| float | 4字节 | 单精度浮点 |
| double | 8字节 | 双精度浮点 |
| char | 1字节 | 字符 |

```c
printf("%d\n", sizeof(int));    // 4
printf("%d\n", sizeof(double)); // 8
```

## printf 格式符

```
%d   → int
%f   → float/double
%lf  → double（scanf中用）
%c   → char
%s   → 字符串
%ld  → long
%p   → 指针地址
```

---

**问题：** 用 `%d` 打印 `float`，输出乱码
```c
float a = 3.14;
printf("%d\n", a);  // 错误，格式符与类型不匹配
```
**解决：** 改为 `%f`
```c
printf("%f\n", a);
```

---

**问题：** 浮点数相等判断失败
```c
float a = 0.1 + 0.2;
if (a == 0.3) { ... }  // 几乎永远为false
```
**解决：** 判断差值是否小于精度阈值
```c
if (fabs(a - 0.3) < 1e-6) { ... }
```
参考：http://blog.csdn.net/zl_best/article/details/8156337
