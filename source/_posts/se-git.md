---
title: 软件工程 -- Git 与版本控制
date: 2017-05-12
tags: [软件工程, Git]
categories:
  - 计算机基础
  - 软件工程
---

版本控制是多人协作的基础。  
即使一个人写项目，也应该用 Git。

没有版本控制时，文件名可能会变成：

```text
final.doc
final2.doc
final_really_final.doc
```

这很危险，也很难回退。

## Git 解决什么

Git 可以记录每次修改：

- 改了什么
- 谁改的
- 什么时候改的
- 为什么改

也可以回到之前版本。

常用命令：

```bash
git status
git add
git commit
git log
git diff
git branch
git merge
```

## commit 信息

commit 不只是保存代码，也是在写项目历史。

好的 commit message 应该说明这次修改做了什么。

比如：

```text
add student score query
fix login validation
```

比 `update`、`修改一下` 更清楚。

## 分支

分支适合做新功能或实验。  
主分支保持稳定，新功能在分支开发，完成后再合并。

key：

- Git 是项目历史记录。
- 小步提交比一次提交一大堆更容易管理。
- commit message 要让以后的人看得懂。

