---
title: 操作系统 18 - Linux 进程初步
date: 2017-05-31
tags: [操作系统, Linux]
categories:
  - 计算机基础
  - 操作系统
---

学完进程概念后，用 Linux 命令看一下会更直观。

常用命令：

```bash
ps
top
kill
jobs
fg
bg
```

`ps` 可以查看当前进程。  
`top` 可以动态查看系统负载。  
`kill` 可以给进程发送信号。

Linux 里每个进程都有 pid。父子进程之间也有关系。  
Shell 启动命令时，通常会创建子进程去执行。

概念课里讲的进程状态、调度、阻塞，在真实系统中都能找到影子。

