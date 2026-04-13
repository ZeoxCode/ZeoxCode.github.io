---
title: 数据结构 05 - 树与二叉树
date: 2016-11-21
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 二叉树定义

```cpp
typedef struct BiNode {
    int data;
    struct BiNode *lchild, *rchild;
} BiNode, *BiTree;
```

## 三种遍历（递归）

```cpp
void PreOrder(BiTree T) {   // 前序：根左右
    if (!T) return;
    cout << T->data;
    PreOrder(T->lchild);
    PreOrder(T->rchild);
}

void InOrder(BiTree T) {    // 中序：左根右
    if (!T) return;
    InOrder(T->lchild);
    cout << T->data;
    InOrder(T->rchild);
}

void PostOrder(BiTree T) {  // 后序：左右根
    if (!T) return;
    PostOrder(T->lchild);
    PostOrder(T->rchild);
    cout << T->data;
}
```

## 层序遍历

```cpp
void LevelOrder(BiTree T) {
    queue<BiTree> q;
    q.push(T);
    while (!q.empty()) {
        BiTree p = q.front(); q.pop();
        cout << p->data;
        if (p->lchild) q.push(p->lchild);
        if (p->rchild) q.push(p->rchild);
    }
}
```

## 重要性质

- 第 i 层最多 **2^(i-1)** 个节点
- 深度为 k 的二叉树最多 **2^k - 1** 个节点
- **n₀ = n₂ + 1**（叶子节点数 = 度为2的节点数 + 1）
- 完全二叉树中，节点 i 的左孩子是 `2i`，右孩子是 `2i+1`（1-based）

## 哈夫曼树

构造：每次取权值最小的两棵树合并，新节点权值为两者之和，重复直到只剩一棵树。

```
权值：{1, 3, 5, 7}
第1次：合并 1,3 → 4
第2次：合并 4,5 → 9
第3次：合并 7,9 → 16
WPL = 1×3 + 3×3 + 5×2 + 7×2 = 36
```

哈夫曼树没有度为 1 的节点。

## 常见坑

- 前序+中序可以唯一确定一棵二叉树，后序+中序也可以，**前序+后序不能**
- BST 的中序遍历结果是**有序序列**

## 经典例题

**已知前序 ABDCE，中序 BDACE，求后序：**

```
前序第一个 A 是根
中序中 A 左边 BD 是左子树，右边 CE 是右子树
左子树前序 BD → B 是根，中序 BD → D 是 B 的右孩子
右子树前序 CE → C 是根，中序 CE → E 是 C 的右孩子

后序：D B E C A
```
