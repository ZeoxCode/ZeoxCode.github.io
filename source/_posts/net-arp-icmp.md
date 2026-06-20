---
title: 计算机网络 -- ARP 与 ICMP
date: 2017-05-04
tags: [计算机网络]
categories:
  - 计算机基础
  - 计算机网络
---

ARP 和 ICMP 都很小，但很关键。

不能忘的点：

- ARP 用 IP 地址查询 MAC 地址。
- ICMP 用来传递网络控制和错误信息。
- `ping` 主要用的就是 ICMP。

ARP 只在局域网里工作。  
如果目标不在同一网段，主机会先找网关的 MAC 地址。

一句话：  
ARP 解决“下一跳 MAC 是谁”，ICMP 解决“网络状态怎么反馈”。

