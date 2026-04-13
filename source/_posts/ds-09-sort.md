---
title: 数据结构 09 - 排序
date: 2016-12-20
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 插入排序 O(n²)

```cpp
void InsertSort(int a[], int n) {
    for (int i = 1; i < n; i++) {
        int key = a[i], j = i - 1;
        while (j >= 0 && a[j] > key) {
            a[j+1] = a[j];
            j--;
        }
        a[j+1] = key;
    }
}
```

## 希尔排序 O(n^1.3)

```cpp
void ShellSort(int a[], int n) {
    for (int gap = n/2; gap > 0; gap /= 2)
        for (int i = gap; i < n; i++) {
            int key = a[i], j = i - gap;
            while (j >= 0 && a[j] > key) {
                a[j+gap] = a[j];
                j -= gap;
            }
            a[j+gap] = key;
        }
}
```

## 快速排序 O(nlogn) 平均

```cpp
int Partition(int a[], int low, int high) {
    int pivot = a[low];
    while (low < high) {
        while (low < high && a[high] >= pivot) high--;
        a[low] = a[high];
        while (low < high && a[low] <= pivot) low++;
        a[high] = a[low];
    }
    a[low] = pivot;
    return low;
}

void QuickSort(int a[], int low, int high) {
    if (low < high) {
        int p = Partition(a, low, high);
        QuickSort(a, low, p-1);
        QuickSort(a, p+1, high);
    }
}
```

## 归并排序 O(nlogn)

```cpp
void Merge(int a[], int tmp[], int l, int mid, int r) {
    int i = l, j = mid+1, k = l;
    while (i <= mid && j <= r)
        tmp[k++] = (a[i] <= a[j]) ? a[i++] : a[j++];
    while (i <= mid) tmp[k++] = a[i++];
    while (j <= r)   tmp[k++] = a[j++];
    for (int x = l; x <= r; x++) a[x] = tmp[x];
}

void MergeSort(int a[], int tmp[], int l, int r) {
    if (l >= r) return;
    int mid = (l + r) / 2;
    MergeSort(a, tmp, l, mid);
    MergeSort(a, tmp, mid+1, r);
    Merge(a, tmp, l, mid, r);
}
```

## 堆排序 O(nlogn)

```cpp
void Heapify(int a[], int n, int i) {
    int largest = i, l = 2*i+1, r = 2*i+2;
    if (l < n && a[l] > a[largest]) largest = l;
    if (r < n && a[r] > a[largest]) largest = r;
    if (largest != i) {
        swap(a[i], a[largest]);
        Heapify(a, n, largest);
    }
}

void HeapSort(int a[], int n) {
    for (int i = n/2-1; i >= 0; i--) Heapify(a, n, i);
    for (int i = n-1; i > 0; i--) {
        swap(a[0], a[i]);
        Heapify(a, i, 0);
    }
}
```

## 排序算法对比

| 算法 | 平均 | 最坏 | 空间 | 稳定 |
|------|------|------|------|------|
| 插入排序 | O(n²) | O(n²) | O(1) | 稳定 |
| 希尔排序 | O(n^1.3) | O(n²) | O(1) | 不稳定 |
| 快速排序 | O(nlogn) | O(n²) | O(logn) | 不稳定 |
| 归并排序 | O(nlogn) | O(nlogn) | O(n) | 稳定 |
| 堆排序 | O(nlogn) | O(nlogn) | O(1) | 不稳定 |

## 常见坑

- 快速排序最坏情况：数组已有序，pivot 每次都是最值，退化为 O(n²)
- 归并排序需要额外 O(n) 空间，堆排序不需要
- **稳定**：插入、冒泡、归并；**不稳定**：希尔、快速、堆
- 堆排序建堆从 `n/2-1` 开始向下调整，不是从 `n-1`
