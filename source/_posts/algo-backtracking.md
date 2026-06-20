---
title: 算法设计与分析 -- 回溯
date: 2017-06-28
tags: [算法, 回溯, 搜索]
categories:
  - 计算机基础
  - 算法设计与分析
---

回溯是一种搜索方法。

它适合解决：

- 枚举所有方案
- 找满足条件的方案
- 组合、排列、子集
- 棋盘类问题

回溯的核心动作是：

```text
选择 -> 递归 -> 撤销选择
```

## 子集问题

给 `[1,2,3]`，枚举所有子集。

```cpp
vector<vector<int>> ans;
vector<int> path;

void dfs(int idx) {
    if (idx == n) {
        ans.push_back(path);
        return;
    }

    // 不选 a[idx]
    dfs(idx + 1);

    // 选 a[idx]
    path.push_back(a[idx]);
    dfs(idx + 1);
    path.pop_back();
}
```

这里 `path.pop_back()` 就是撤销选择。

## 排列问题

排列和子集不同，每个位置要从未使用元素中选一个。

```cpp
void dfs() {
    if (path.size() == n) {
        ans.push_back(path);
        return;
    }

    for (int i = 0; i < n; i++) {
        if (used[i]) continue;
        used[i] = true;
        path.push_back(a[i]);

        dfs();

        path.pop_back();
        used[i] = false;
    }
}
```

## 剪枝

回溯如果枚举所有情况，可能很慢。  
剪枝就是提前停止不可能成功的分支。

比如求和问题：

```text
当前和已经超过 target
```

如果所有数都是正数，就可以直接返回，不必继续递归。

## N 皇后

N 皇后是回溯经典题。  
每一行放一个皇后，检查列、主对角线、副对角线是否冲突。

状态：

- 当前放到第几行
- 哪些列被占
- 哪些对角线被占

每行尝试所有列，合法就继续下一行。

## 回溯模板

大致模板：

```cpp
void dfs(状态) {
    if (到达终点) {
        记录答案;
        return;
    }

    for (选择 : 当前可选集合) {
        if (不合法) continue;
        做选择;
        dfs(新状态);
        撤销选择;
    }
}
```

key：

- 回溯本质是深度优先搜索。
- 关键是选择、递归、撤销。
- 剪枝决定搜索能不能跑得动。

