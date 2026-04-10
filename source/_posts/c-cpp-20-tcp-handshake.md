---
title: C/C++学习总结（二十）——TCP三次握手模拟
date: 2016-11-09
categories:
  - 全栈开发
  - 后端
tags:
  - C
  - 网络
---

## TCP 三次握手过程

```
客户端                    服务端
  |  ——— SYN(seq=x) ———→  |   客户端：CLOSED → SYN_SENT
  |  ←— SYN+ACK(ack=x+1) —|   服务端：LISTEN → SYN_RCVD
  |  ——— ACK(ack=y+1) ———→ |   双方：→ ESTABLISHED
```

## 状态定义

```c
typedef enum {
    CLOSED,
    LISTEN,
    SYN_SENT,
    SYN_RCVD,
    ESTABLISHED
} TCPState;
```

## 报文标志位

```c
#define SYN 0x02
#define ACK 0x10
#define FIN 0x01
```

## 核心模拟逻辑

```c
typedef struct {
    unsigned int seq;
    unsigned int ack;
    unsigned char flags;
} Packet;

// 客户端发起握手
void client_handshake(int sock) {
    Packet pkt;

    // 第一次握手：发SYN
    pkt.seq   = rand();
    pkt.flags = SYN;
    send(sock, &pkt, sizeof(pkt), 0);
    printf("Client → SYN sent, seq=%u\n", pkt.seq);

    // 第二次握手：收SYN+ACK
    unsigned int client_seq = pkt.seq;
    recv(sock, &pkt, sizeof(pkt), 0);
    printf("Client ← SYN+ACK received\n");

    // 第三次握手：发ACK
    Packet ack_pkt;
    ack_pkt.seq   = client_seq + 1;
    ack_pkt.ack   = pkt.seq + 1;
    ack_pkt.flags = ACK;
    send(sock, &ack_pkt, sizeof(ack_pkt), 0);
    printf("Client → ACK sent\n");
    printf("Client state: ESTABLISHED\n");
}

// 服务端响应握手
void server_handshake(int sock) {
    Packet pkt;

    // 收第一次握手
    recv(sock, &pkt, sizeof(pkt), 0);
    printf("Server ← SYN received, seq=%u\n", pkt.seq);

    // 第二次握手：发SYN+ACK
    unsigned int client_seq = pkt.seq;
    Packet synack;
    synack.seq   = rand();
    synack.ack   = client_seq + 1;
    synack.flags = SYN | ACK;
    send(sock, &synack, sizeof(synack), 0);
    printf("Server → SYN+ACK sent\n");

    // 收第三次握手
    recv(sock, &pkt, sizeof(pkt), 0);
    printf("Server ← ACK received\n");
    printf("Server state: ESTABLISHED\n");
}
```

---

**问题：** 直接用结构体发送，两端字节序不一致导致seq/ack值错误
**解决：** 发送前用 `htonl()` 转换，接收后用 `ntohl()` 还原
```c
synack.seq = htonl(rand());
// 接收方：
pkt.seq = ntohl(pkt.seq);
```

---

**问题：** `#pragma pack(1)` 未加，结构体实际大小与预期不符，recv读取字节数对不上
**解决：** 报文结构体加 `#pragma pack(1)`，关闭内存对齐。

---

**问题：** 服务端 accept 阻塞，客户端先跑完退出，connect 失败
**解决：** 确保服务端先启动并进入 listen 状态，再启动客户端。
