---
title: 操作系统 05 - fork、exec 与 wait
date: 2017-04-26
tags: [操作系统, Linux]
categories:
  - 计算机基础
  - 操作系统
---

Unix 里创建进程的方式很有意思。

`fork` 会复制出一个子进程。子进程和父进程几乎一样，只是返回值不同：

- 父进程中返回子进程 pid
- 子进程中返回 0

`exec` 用一个新程序替换当前进程的内容。  
`wait` 用来等待子进程结束。

大概流程：

```c
pid = fork();
if (pid == 0) {
    exec(...);
} else {
    wait(...);
}
```

Shell 执行命令时，本质上就是不断 fork 子进程，然后在子进程里 exec 对应程序。

