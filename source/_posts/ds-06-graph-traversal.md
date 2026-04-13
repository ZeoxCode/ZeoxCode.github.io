---
title: 数据结构 06 - 图（上）存储与遍历
date: 2016-11-28
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 邻接矩阵

```cpp
#define MAXV 100
typedef struct {
    int edge[MAXV][MAXV];
    int vexNum, edgeNum;
} MGraph;
// 无向图：edge[i][j] == edge[j][i]
// 空间 O(V²)，适合稠密图
```

## 邻接表

```cpp
typedef struct ArcNode {
    int adjvex;
    struct ArcNode* next;
} ArcNode;

typedef struct {
    int data;
    ArcNode* first;
} VNode;

typedef struct {
    VNode adjList[MAXV];
    int vexNum, edgeNum;
} ALGraph;
// 空间 O(V+E)，适合稀疏图
```

## DFS

```cpp
bool visited[MAXV] = {false};

void DFS(ALGraph G, int v) {
    visited[v] = true;
    cout << v;
    ArcNode* p = G.adjList[v].first;
    while (p) {
        if (!visited[p->adjvex])
            DFS(G, p->adjvex);
        p = p->next;
    }
}
```

## BFS

```cpp
void BFS(ALGraph G, int v) {
    queue<int> q;
    visited[v] = true;
    q.push(v);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        cout << u;
        ArcNode* p = G.adjList[u].first;
        while (p) {
            if (!visited[p->adjvex]) {
                visited[p->adjvex] = true;
                q.push(p->adjvex);
            }
            p = p->next;
        }
    }
}
```

## 常见坑

- DFS 用**递归**（栈），BFS 用**队列**，不要混
- 邻接矩阵判断边是否存在 O(1)，邻接表需要遍历 O(度)
- 非连通图需要对每个未访问节点都调用一次 DFS/BFS
