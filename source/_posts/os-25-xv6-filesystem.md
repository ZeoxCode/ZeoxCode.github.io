---
title: 操作系统 25 - xv6 文件系统
date: 2017-06-16
tags: [操作系统, xv6]
categories:
  - 计算机基础
  - 操作系统
---

xv6 文件系统也用了 inode。

磁盘大概被分成：

- boot block
- super block
- log
- inode blocks
- bitmap
- data blocks

文件读写时，不是直接按文件名找数据块，而是先通过目录找到 inode，再根据 inode 找数据块。

日志区用来保证崩溃一致性。  
如果写文件写到一半系统崩了，日志可以帮助文件系统恢复到一个比较一致的状态。

文件系统看起来简单，其实要同时考虑组织、缓存和安全写入。

