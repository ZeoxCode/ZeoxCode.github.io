---
title: 算法设计与分析 -- 快速排序
date: 2017-06-05
tags: [算法, 排序, 分治]
categories:
  - 计算机基础
  - 算法设计与分析
---

快速排序也是分治，但和归并排序不一样。

归并排序是：

```text
先递归排，再合并。
```

快速排序是：

```text
先划分，再递归排。
```

## 核心思想

选一个基准 pivot，把数组分成两部分：

```text
左边 <= pivot
右边 >= pivot
```

然后递归排序左右两边。

## 分区过程

一种常见写法：

```cpp
int partition(vector<int>& a, int l, int r) {
    int pivot = a[r];
    int i = l;
    for (int j = l; j < r; j++) {
        if (a[j] < pivot) {
            swap(a[i], a[j]);
            i++;
        }
    }
    swap(a[i], a[r]);
    return i;
}
```

递归：

```cpp
void quickSort(vector<int>& a, int l, int r) {
    if (l >= r) return;
    int p = partition(a, l, r);
    quickSort(a, l, p - 1);
    quickSort(a, p + 1, r);
}
```

## 复杂度

如果每次都能接近平均分：

```text
T(n) = 2T(n/2) + O(n)
=> O(n log n)
```

如果每次 pivot 都选到最大或最小：

```text
T(n) = T(n-1) + O(n)
=> O(n^2)
```

所以快速排序平均很好，最坏可能很差。

## 为什么实际中常用

虽然最坏 `O(n^2)`，快速排序仍然常用，因为：

- 平均性能好。
- 原地排序，额外空间少。
- Cache 友好。
- 常数小。

为了避免最坏情况，可以随机选 pivot：

```cpp
int idx = l + rand() % (r - l + 1);
swap(a[idx], a[r]);
```

## 和归并排序对比

| 项目 | 归并排序 | 快速排序 |
|---|---|---|
| 平均时间 | O(n log n) | O(n log n) |
| 最坏时间 | O(n log n) | O(n^2) |
| 额外空间 | O(n) | 平均 O(log n) |
| 稳定性 | 稳定 | 通常不稳定 |
| 实际表现 | 稳 | 通常很快 |

## 一个坑

如果数组里有大量重复元素，普通双路 partition 可能表现不好。  
可以用三路快排，把数组分成：

```text
< pivot
= pivot
> pivot
```

这对重复元素很多的情况更好。

key：

- 快排核心是 partition。
- 平均 `O(n log n)`，最坏 `O(n^2)`。
- 随机 pivot 和三路划分能提高稳健性。

