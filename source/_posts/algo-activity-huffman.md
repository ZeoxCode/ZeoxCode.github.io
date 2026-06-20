---
title: 算法设计与分析 -- 活动选择与 Huffman 编码
date: 2017-06-12
tags: [算法, 贪心, Huffman]
categories:
  - 计算机基础
  - 算法设计与分析
---

贪心算法最经典的两个例子：活动选择和 Huffman 编码。

一个是区间问题，一个是编码问题。

## 活动选择

有很多活动，每个活动有开始时间和结束时间。  
同一时间只能参加一个活动，问最多能参加多少个。

活动：

```text
A: 1-4
B: 3-5
C: 0-6
D: 5-7
E: 8-9
```

贪心策略：

> 每次选择结束时间最早的活动。

为什么不是选开始最早？  
开始早可能占用很长时间，反而阻碍后面活动。

为什么选结束最早合理？  
因为它给剩下活动留下最多时间。

## 代码思路

先按结束时间排序：

```cpp
sort(events.begin(), events.end(), [](auto& a, auto& b) {
    return a.end < b.end;
});

int lastEnd = -INF;
int ans = 0;
for (auto& e : events) {
    if (e.start >= lastEnd) {
        ans++;
        lastEnd = e.end;
    }
}
```

复杂度主要来自排序：

```text
O(n log n)
```

## Huffman 编码

Huffman 编码解决的是变长编码问题。

如果一个字符出现频率高，就给它短编码。  
如果出现频率低，就给它长编码。

这样整体编码长度更短。

## 为什么要前缀编码

编码不能有歧义。

比如：

```text
A = 0
B = 01
```

那么读到 `01` 时，不知道是 `A` 后面跟 `1`，还是 `B`。

Huffman 编码要求任何一个字符编码都不是另一个字符编码的前缀。

## Huffman 贪心过程

每次选频率最小的两个节点合并。

例子：

```text
a:5, b:9, c:12, d:13, e:16, f:45
```

先合并 5 和 9，得到 14。  
再从新集合里继续选最小的两个合并。

用优先队列实现：

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
for (int f : freq) pq.push(f);

int cost = 0;
while (pq.size() > 1) {
    int a = pq.top(); pq.pop();
    int b = pq.top(); pq.pop();
    cost += a + b;
    pq.push(a + b);
}
```

这个 `cost` 就是带权路径长度。

## 为什么是贪心

频率最低的两个字符应该放在最深层。  
它们的编码最长，但因为出现少，所以代价小。

每次合并最小的两个，本质是在构造最优前缀编码树。

key：

- 活动选择贪心选结束最早。
- Huffman 每次合并频率最小的两个节点。
- 贪心题关键是证明局部选择是安全的。

