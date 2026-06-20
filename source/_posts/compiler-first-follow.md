---
title: 编译原理 -- FIRST 与 FOLLOW
date: 2017-04-05
tags: [编译原理]
categories:
  - 计算机基础
  - 编译原理
---

FIRST 和 FOLLOW 是语法分析里比较像“做题”的部分。

刚学的时候容易背规则，但最好先理解它们要解决的问题：

> 分析器看到下一个 token 时，应该选择哪条产生式？

## FIRST 集

FIRST(X) 表示从 X 推导出来的串，开头可能出现哪些终结符。

例子：

```text
E -> T E'
T -> id | ( E )
```

那么：

```text
FIRST(T) = { id, ( }
FIRST(E) = { id, ( }
```

因为 E 先推出 T，而 T 可以以 `id` 或 `(` 开头。

## FOLLOW 集

FOLLOW(A) 表示在某些句型里，A 后面可能跟哪些终结符。

比如：

```text
F -> ( E )
```

这里 E 后面可以跟 `)`，所以 `)` 属于 FOLLOW(E)。

如果 E 是开始符号，通常 `$` 也在 FOLLOW(E) 里，表示输入结束。

## 为什么需要它

LL(1) 分析表要靠 FIRST 和 FOLLOW 构造。

特别是产生式可以推出空串 ε 时，要看 FOLLOW 来决定什么时候使用这条空产生式。

## 小例子

```text
E' -> + T E' | ε
```

如果下一个 token 是 `+`，选第一条。  
如果下一个 token 是 `)` 或输入结束，说明加法部分没有了，选 ε。

key：

- FIRST 看“能以什么开头”。
- FOLLOW 看“后面可能跟什么”。
- ε 产生式通常要结合 FOLLOW 判断。

