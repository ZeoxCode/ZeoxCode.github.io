---
title: 数据结构 01 - 绪论与算法复杂度
date: 2016-10-25
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 时间复杂度

增长速度从小到大：

```
O(1) < O(logn) < O(n) < O(nlogn) < O(n²) < O(2ⁿ)
```

只取最高次项，忽略系数：

```cpp
// O(n²)
for (int i = 0; i < n; i++)
    for (int j = 0; j < n; j++)
        sum++;

// O(logn)
while (n > 1) n /= 2;
```

## 空间复杂度

递归的空间复杂度等于递归深度：

```cpp
int f(int n) {
    if (n <= 1) return 1;
    return n * f(n-1);  // 空间 O(n)
}
```

## 常见坑

```cpp
// 内层从 i 开始，仍然是 O(n²)
for (int i = 0; i < n; i++)
    for (int j = i; j < n; j++)
        sum++;
// 执行次数 = n(n+1)/2，量级不变
```

`O(n + n²)` 不是 `O(2n²)`，直接取最高次项写 `O(n²)`。
