---
title: 数据结构 03 - 栈与队列
date: 2016-11-07
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 顺序栈

```cpp
#define MAXSIZE 100
typedef struct {
    int data[MAXSIZE];
    int top;  // 初始化为 -1
} SqStack;

void Push(SqStack &S, int e) {
    S.data[++S.top] = e;
}

int Pop(SqStack &S) {
    return S.data[S.top--];
}
```

## 链式队列

```cpp
typedef struct QNode {
    int data;
    struct QNode* next;
} QNode;

typedef struct {
    QNode *front, *rear;
} LinkQueue;

void EnQueue(LinkQueue &Q, int e) {
    QNode* p = new QNode;
    p->data = e;
    p->next = nullptr;
    Q.rear->next = p;
    Q.rear = p;
}

int DeQueue(LinkQueue &Q) {
    QNode* p = Q.front->next;
    int e = p->data;
    Q.front->next = p->next;
    if (Q.rear == p) Q.rear = Q.front;  // 队列变空
    delete p;
    return e;
}
```

## 循环队列

```cpp
#define MAXSIZE 100
typedef struct {
    int data[MAXSIZE];
    int front, rear;  // 初始均为 0
} SqQueue;

// 判满：(rear+1) % MAXSIZE == front
// 判空：front == rear
// 队列长度：(rear - front + MAXSIZE) % MAXSIZE

void EnQueue(SqQueue &Q, int e) {
    if ((Q.rear + 1) % MAXSIZE == Q.front) return;  // 满
    Q.data[Q.rear] = e;
    Q.rear = (Q.rear + 1) % MAXSIZE;
}
```

## 常见坑

- 顺序栈 `top` 初始化为 `-1`，不是 `0`（严蔚敏版）
- 循环队列**少用一个空间**来区分满和空，实际容量是 `MAXSIZE-1`
- 链式队列删除最后一个元素时，`rear` 要指回 `front`，否则 `rear` 悬空
