---
title: 算法设计与分析 -- 归并排序
date: 2017-06-02
tags: [算法, 排序, 分治]
categories:
  - 计算机基础
  - 算法设计与分析
---

归并排序是分治法最经典的例子。

思想：

```text
先把数组分成两半，分别排好序，再把两个有序数组合并。
```

## 过程

数组：

```text
8 4 5 7 1 3 6 2
```

不断拆：

```text
8 4 5 7 | 1 3 6 2
8 4 | 5 7 | 1 3 | 6 2
8 | 4 | 5 | 7 | 1 | 3 | 6 | 2
```

单个元素天然有序。  
然后合并：

```text
4 8 | 5 7 | 1 3 | 2 6
4 5 7 8 | 1 2 3 6
1 2 3 4 5 6 7 8
```

## 合并两个有序数组

这是归并排序的核心。

```cpp
void merge(vector<int>& a, int l, int mid, int r) {
    vector<int> tmp;
    int i = l, j = mid + 1;
    while (i <= mid && j <= r) {
        if (a[i] <= a[j]) tmp.push_back(a[i++]);
        else tmp.push_back(a[j++]);
    }
    while (i <= mid) tmp.push_back(a[i++]);
    while (j <= r) tmp.push_back(a[j++]);
    for (int k = 0; k < tmp.size(); k++) {
        a[l + k] = tmp[k];
    }
}
```

递归：

```cpp
void mergeSort(vector<int>& a, int l, int r) {
    if (l >= r) return;
    int mid = l + (r - l) / 2;
    mergeSort(a, l, mid);
    mergeSort(a, mid + 1, r);
    merge(a, l, mid, r);
}
```

## 复杂度

递推式：

```text
T(n) = 2T(n/2) + O(n)
```

所以：

```text
O(n log n)
```

空间复杂度是：

```text
O(n)
```

因为合并时需要临时数组。

## 稳定性

归并排序是稳定排序。  
如果两个元素相等，原来在前面的仍然在前面。

代码里这一句很关键：

```cpp
if (a[i] <= a[j])
```

相等时先取左边，可以保持稳定。

## 什么时候适合归并排序

优点：

- 时间复杂度稳定 `O(n log n)`。
- 稳定。
- 适合链表排序。
- 适合外部排序，因为可以分块合并。

缺点：

- 需要额外空间。
- 对数组原地排序不如快速排序常用。

key：

- 归并排序是分治：先排子数组，再合并。
- 时间稳定 `O(n log n)`，空间 `O(n)`。
- 它是稳定排序。

