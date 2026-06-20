---
title: 算法设计与分析 -- 字符串匹配
date: 2017-07-03
tags: [算法, 字符串, KMP]
categories:
  - 计算机基础
  - 算法设计与分析
---

字符串匹配问题：

> 在文本 T 中找模式串 P 出现的位置。

最朴素方法是暴力匹配。

## 暴力匹配

```cpp
for (int i = 0; i + m <= n; i++) {
    bool ok = true;
    for (int j = 0; j < m; j++) {
        if (T[i+j] != P[j]) {
            ok = false;
            break;
        }
    }
    if (ok) return i;
}
```

最坏复杂度：

```text
O(nm)
```

如果文本和模式串都很长，就慢。

## KMP 的想法

KMP 的核心是：

> 匹配失败时，不要把已经知道的信息丢掉。

比如模式串：

```text
ababaca
```

如果已经匹配了一部分，失败后可以根据前缀和后缀关系跳转，而不是从头开始。

## next / prefix 数组

KMP 会预处理模式串，计算每个位置之前的最长相等前后缀长度。

例如：

```text
P = ababa
```

前缀和后缀：

```text
a
ab
aba
abab
```

最长相等前后缀可以帮助模式串右移。

## 复杂度

KMP 预处理模式串：

```text
O(m)
```

匹配文本：

```text
O(n)
```

总复杂度：

```text
O(n + m)
```

## 其他字符串算法

字符串匹配不只有 KMP：

- Rabin-Karp：用哈希。
- Boyer-Moore：从右向左匹配，实际很快。
- Trie：多模式前缀匹配。
- AC 自动机：多模式串匹配。

不同算法适合不同场景。

## 什么时候用什么

单模式精确匹配：KMP。  
大量模式串：Trie 或 AC 自动机。  
需要快速工程实现：很多语言库里的 find 已经很强。

key：

- 暴力匹配最坏 `O(nm)`。
- KMP 利用前缀后缀信息避免回退文本指针。
- KMP 总复杂度 `O(n+m)`。

