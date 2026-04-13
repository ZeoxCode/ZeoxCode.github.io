---
title: 数据结构 04 - 串与KMP
date: 2016-11-14
tags: [数据结构, C++]
categories:
  - 计算机基础
  - 数据结构
---

## 暴力匹配

```cpp
int BruteForce(string S, string T) {
    int i = 0, j = 0;
    while (i < (int)S.size() && j < (int)T.size()) {
        if (S[i] == T[j]) { i++; j++; }
        else { i = i - j + 1; j = 0; }  // i回退，j归零
    }
    return (j == (int)T.size()) ? i - j : -1;
}
// 时间复杂度 O(m*n)
```

## KMP算法

核心：匹配失败时，**i 不回退**，j 根据 next 数组跳转。

```cpp
// 求 next 数组
void getNext(string T, int next[]) {
    next[0] = -1;
    int k = -1, j = 0;
    while (j < (int)T.size() - 1) {
        if (k == -1 || T[j] == T[k]) {
            next[++j] = ++k;
        } else {
            k = next[k];
        }
    }
}

// KMP 匹配
int KMP(string S, string T) {
    int next[T.size()];
    getNext(T, next);
    int i = 0, j = 0;
    while (i < (int)S.size() && j < (int)T.size()) {
        if (j == -1 || S[i] == T[j]) { i++; j++; }
        else j = next[j];
    }
    return (j == (int)T.size()) ? i - j : -1;
}
// 时间复杂度 O(m+n)
```

## next 数组推导示例

```
T =   a  b  a  b  c
idx=  0  1  2  3  4

next= -1  0  0  1  2

含义：next[j] = 模式串T[0..j-1]的最长公共前后缀长度
失配时 j 跳到 next[j]，相当于模式串右移
```

## 常见坑

- next 数组下标从 0 还是 1 开始，严蔚敏版从 1 开始，代码实现时容易对不上
- 失配时是 `j = next[j]`，不是 `j--`
- KMP 相比暴力的优势在于**主串指针 i 不回退**，适合大文本匹配
