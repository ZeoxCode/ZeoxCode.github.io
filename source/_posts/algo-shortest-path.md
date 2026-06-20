---
title: 算法设计与分析 -- 最短路径
date: 2017-06-16
tags: [算法, 图, 最短路径]
categories:
  - 计算机基础
  - 算法设计与分析
---

最短路径问题问的是：

> 图中从一个点到另一个点，最小代价是多少。

不同条件下要用不同算法。

## BFS：无权图最短路

如果每条边权重都一样，可以用 BFS。

BFS 一层一层扩展。  
第一次到达某个点时，走过的边数就是最少的。

适合：

```text
无权图 / 所有边权相同
```

复杂度：

```text
O(V + E)
```

## Dijkstra：非负权单源最短路

Dijkstra 适合边权非负的图。

思路：

1. `dist[s] = 0`，其他为无穷大。
2. 每次选当前未确定点中 dist 最小的点。
3. 用它更新相邻点。

用优先队列实现：

```cpp
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
dist[s] = 0;
pq.push({0, s});

while (!pq.empty()) {
    auto [d, u] = pq.top(); pq.pop();
    if (d != dist[u]) continue;
    for (auto [v, w] : g[u]) {
        if (dist[v] > dist[u] + w) {
            dist[v] = dist[u] + w;
            pq.push({dist[v], v});
        }
    }
}
```

注意：Dijkstra 不能处理负权边。

## Bellman-Ford：能处理负权边

Bellman-Ford 可以处理负权边，还能检测负环。

核心是反复松弛所有边：

```text
最多进行 V-1 轮
```

如果第 V 轮还能更新，说明存在负环。

复杂度：

```text
O(VE)
```

比 Dijkstra 慢，但更通用。

## Floyd：多源最短路

Floyd 求任意两点之间最短路。

状态：

```text
dist[i][j] = i 到 j 的当前最短距离
```

转移：

```cpp
for (int k = 0; k < n; k++)
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
```

复杂度：

```text
O(n^3)
```

适合点数不太大的图。

## 怎么选算法

| 场景 | 算法 |
|---|---|
| 无权图 | BFS |
| 非负权单源 | Dijkstra |
| 有负权边 | Bellman-Ford |
| 任意两点最短路 | Floyd |

key：

- 最短路算法要看边权条件。
- Dijkstra 要求非负权。
- Floyd 简单但 `O(n^3)`，适合小规模多源最短路。

