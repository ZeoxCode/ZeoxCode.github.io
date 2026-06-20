---
title: 编译原理 -- 递归下降分析
date: 2017-04-03
tags: [编译原理]
categories:
  - 计算机基础
  - 编译原理
---

递归下降分析是比较容易理解的一种语法分析方法。

它的思路很直观：

> 每个非终结符写成一个函数。

比如文法：

```text
Expr -> Term Rest
Rest -> + Term Rest | ε
Term -> id
```

就可以写成几个函数：

```text
parseExpr()
parseRest()
parseTerm()
```

## 一个简单过程

输入：

```text
a + b
```

`parseExpr` 先解析一个 Term，也就是 `a`。  
然后 `parseRest` 看到 `+`，继续解析下一个 Term，也就是 `b`。  
最后没有更多 `+`，结束。

## 左递归问题

递归下降最怕左递归。

比如：

```text
E -> E + T | T
```

如果 `parseE()` 一上来又调用 `parseE()`，就会无限递归。

所以 LL 分析通常要消除左递归。

把它改成：

```text
E  -> T E'
E' -> + T E' | ε
```

这样就能从左到右分析。

key：

- 递归下降把语法规则写成函数。
- 它直观，适合手写简单 parser。
- 左递归会导致无限递归，需要消除。

