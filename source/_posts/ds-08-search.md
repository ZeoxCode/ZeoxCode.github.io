---
title: 数据结构 08 - 查找
date: 2016-12-12
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 二分查找

```cpp
int BinarySearch(int a[], int n, int key) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;  // 防溢出
        if (a[mid] == key) return mid;
        else if (a[mid] < key) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
// 时间复杂度 O(logn)，要求有序
```

## 二叉搜索树（BST）

```cpp
typedef struct BSTNode {
    int data;
    struct BSTNode *left, *right;
} BSTNode;

BSTNode* Insert(BSTNode* root, int key) {
    if (!root) {
        BSTNode* p = new BSTNode;
        p->data = key;
        p->left = p->right = nullptr;
        return p;
    }
    if (key < root->data) root->left = Insert(root->left, key);
    else if (key > root->data) root->right = Insert(root->right, key);
    return root;
}
```

BST 中序遍历结果是**有序序列**。
删除有右子树的节点：用**中序后继**（右子树最左节点）替换。

## 散列表

```cpp
// 除留余数法
int Hash(int key, int m) { return key % m; }
```

**开放地址法（线性探测）：**
发生冲突时，依次探测 h+1, h+2, ... 直到找到空位。缺点：产生聚集现象。

**链地址法（拉链法）：**
每个槽位存链表，冲突的元素接在链表后面。查找平均长度 = 装填因子 α = n/m。

## 常见坑

- 二分查找 `mid = (low+high)/2` 在 low+high 溢出时出错，应用 `low + (high-low)/2`
- 哈希表装填因子 α 越大冲突越多，一般保持 α < 0.75
- BST 最坏情况（有序插入）退化成链表，查找变为 O(n)
