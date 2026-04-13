---
title: C/C++学习总结（十一）——结构体
date: 2016-10-08
categories:
  - 编程语言
  - C/C++
tags:
  - C
---

## 定义与使用

```c
typedef struct {
    int id;
    char name[20];
    float score;
} Student;

Student s = {1001, "张三", 89.5};
printf("%d %s %.1f\n", s.id, s.name, s.score);
```

## 指向结构体的指针

```c
Student *p = &s;
printf("%d\n", p->id);    // 用 ->
printf("%d\n", (*p).id);  // 等价写法
```

## 应用：TCP 报文头

```c
#pragma pack(1)  // 关闭内存对齐
typedef struct {
    unsigned short src_port;
    unsigned short dst_port;
    unsigned int   seq_num;
    unsigned int   ack_num;
    unsigned char  data_offset;
    unsigned char  flags;
    unsigned short window;
    unsigned short checksum;
} TCPHeader;
#pragma pack()
```

---

**问题：** 结构体大小不等于各成员大小之和（内存对齐）
```c
struct A {
    char a;   // 1字节
    int b;    // 4字节
};
printf("%d\n", sizeof(struct A));  // 输出8，不是5
```
**解决：** 解析网络报文时用 `#pragma pack(1)` 关闭对齐，保证字段与字节流对应。

---

**问题：** 结构体赋值字符串成员不能用 `=`
```c
Student s;
s.name = "张三";  // 错误，数组不能直接赋值
```
**解决：**
```c
strcpy(s.name, "张三");
```
