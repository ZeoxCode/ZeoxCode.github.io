---
title: 数据结构 07 - 图（下）最短路径与最小生成树
date: 2016-12-05
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## Dijkstra（单源最短路径）

```cpp
void Dijkstra(int G[][MAXV], int n, int src) {
    int dist[MAXV], visited[MAXV] = {0};
    for (int i = 0; i < n; i++) dist[i] = G[src][i];
    dist[src] = 0; visited[src] = 1;

    for (int i = 0; i < n-1; i++) {
        // 找未访问中 dist 最小的
        int u = -1, minD = INT_MAX;
        for (int j = 0; j < n; j++)
            if (!visited[j] && dist[j] < minD)
                { minD = dist[j]; u = j; }
        if (u == -1) break;
        visited[u] = 1;
        // 松弛
        for (int v = 0; v < n; v++)
            if (!visited[v] && G[u][v] != INT_MAX)
                dist[v] = min(dist[v], dist[u] + G[u][v]);
    }
}
// 时间复杂度 O(V²)，不能处理负权边
```

## Floyd（多源最短路径）

```cpp
void Floyd(int dist[][MAXV], int n) {
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}
// 时间复杂度 O(V³)，可处理负权边（不能有负权回路）
```

## Prim（最小生成树）

每次从已选节点集合出发，选权值最小的边扩展。
适合**稠密图**，时间复杂度 O(V²)。

## Kruskal（最小生成树）

将所有边按权值排序，依次选边，用并查集判断是否成环。
适合**稀疏图**，时间复杂度 O(ElogE)。

## 拓扑排序

```cpp
// 基于 BFS（Kahn 算法）
// 1. 统计所有节点入度
// 2. 将入度为 0 的节点入队
// 3. 每次出队一个节点，将其邻居入度减 1，减为 0 则入队
// 4. 若最终输出节点数 < 总节点数，说明有环
```

只针对**有向无环图（DAG）**。

## 常见坑

- Dijkstra **不能处理负权边**
- Floyd 三层循环，**k 必须在最外层**，否则结果错误
- Prim 和 Kruskal 得到的最小生成树权值相同，但边的选择顺序可能不同
- 拓扑排序结果不唯一，有多个入度为 0 的节点时顺序可以不同
