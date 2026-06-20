---
title: 计算机网络 -- Socket 编程
date: 2017-06-08
tags: [计算机网络, Socket]
categories:
  - 计算机基础
  - 计算机网络
---

Socket 是应用程序使用网络的接口。

不能忘的点：

- IP 定位主机，端口定位进程。
- TCP Socket 通常要经历 bind、listen、accept、connect。
- 网络编程本质上还是读写，只是读写对象变成了网络连接。

服务端大概流程：

```
socket -> bind -> listen -> accept -> read/write
```

客户端大概流程：

```
socket -> connect -> read/write
```

这和操作系统里的文件描述符也能接上。

