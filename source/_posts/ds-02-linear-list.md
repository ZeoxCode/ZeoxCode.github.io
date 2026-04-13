---
title: 数据结构 02 - 线性表
date: 2016-11-02
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 顺序表

```cpp
#define MAXSIZE 100
typedef struct {
    int data[MAXSIZE];
    int length;
} SqList;

// 插入：第i个位置（1-based）
bool Insert(SqList &L, int i, int e) {
    if (i < 1 || i > L.length + 1) return false;
    for (int j = L.length; j >= i; j--)
        L.data[j] = L.data[j-1];  // 从后往前移
    L.data[i-1] = e;
    L.length++;
    return true;
}

// 删除
bool Delete(SqList &L, int i) {
    if (i < 1 || i > L.length) return false;
    for (int j = i; j < L.length; j++)
        L.data[j-1] = L.data[j];  // 从前往后移
    L.length--;
    return true;
}
```

## 单链表

```cpp
typedef struct Node {
    int data;
    struct Node* next;
} Node, *LinkList;

// 头插法建表（逆序）
LinkList CreateHead(int a[], int n) {
    LinkList L = new Node;
    L->next = nullptr;
    for (int i = 0; i < n; i++) {
        Node* p = new Node;
        p->data = a[i];
        p->next = L->next;
        L->next = p;
    }
    return L;
}

// 尾插法建表（顺序）
LinkList CreateTail(int a[], int n) {
    LinkList L = new Node;
    Node* tail = L;
    for (int i = 0; i < n; i++) {
        Node* p = new Node;
        p->data = a[i];
        tail->next = p;
        tail = p;
    }
    tail->next = nullptr;
    return L;
}
```

## 常见坑

- 顺序表插入：元素要**从后往前**移，从前往后会覆盖数据
- 链表删除节点：要保留**前驱节点**指针，单链表无法直接找前驱
- 头插法建出来的链表是**逆序**的

## 经典例题

**将链表逆置（不开辟新空间）：**

```cpp
LinkList Reverse(LinkList L) {
    Node *prev = nullptr, *cur = L->next, *next;
    while (cur) {
        next = cur->next;
        cur->next = prev;
        prev = cur;
        cur = next;
    }
    L->next = prev;
    return L;
}
```
