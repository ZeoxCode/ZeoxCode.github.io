---
title: 计算机网络 -- Socket 编程
date: 2017-06-08
tags: [计算机网络, Socket]
categories:
  - 计算机基础
  - 计算机网络
---

前面学的是协议原理。
Socket 编程则是应用程序真正使用网络的方式。

可以把 Socket 理解成：

> 操作系统提供给程序使用网络的一套接口。

## IP 和端口

一台机器上会有很多进程同时使用网络。

IP 地址解决的是：

```
数据送到哪台主机？
```

端口解决的是：

```
数据送到这台主机上的哪个进程？
```

所以一次网络通信通常由五元组确定：

```
源 IP、源端口、目的 IP、目的端口、协议
```

## TCP 服务端流程

TCP 服务端通常这样写：

```
socket -> bind -> listen -> accept -> read/write -> close
```

解释：

- `socket`：创建套接字。
- `bind`：绑定本地 IP 和端口。
- `listen`：开始监听连接。
- `accept`：接受客户端连接。
- `read/write`：收发数据。
- `close`：关闭连接。

## TCP 客户端流程

客户端通常简单一些：

```
socket -> connect -> read/write -> close
```

`connect` 会触发 TCP 三次握手。
连接建立后，程序就可以像读写文件一样读写网络连接。

## 为什么 Socket 和文件很像

在 Unix/Linux 里，Socket 也可以通过文件描述符来操作。
所以网络连接、普通文件、管道，都可以用类似的 `read`、`write` 接口。

这和操作系统里“很多东西都是文件”的思想接上了。

## 经典例题

**问题：一个服务器监听 80 端口，能不能同时服务多个客户端？**

可以。

监听 socket 负责等待新连接。
每次 `accept` 成功后，会返回一个新的连接 socket。服务器用这个新的 socket 和某个客户端通信，监听 socket 继续等待其他连接。

## 不能忘的点

- Socket 是应用程序使用网络的接口。
- IP 定位主机，端口定位进程。
- TCP 服务端核心流程是 bind、listen、accept。

