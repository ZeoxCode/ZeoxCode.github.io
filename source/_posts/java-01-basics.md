---
title: Java 01 - 基础入门
date: 2016-10-03
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 第一个程序

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

编译运行：
```bash
javac HelloWorld.java   # 编译 → 生成 HelloWorld.class（字节码）
java HelloWorld         # JVM 执行字节码
```

## 基本结构

```java
public class Dog {
    // 实例变量（成员变量）
    int size;
    String name;

    // 方法
    void bark() {
        System.out.println("Woof!");
    }

    void bark(int times) {          // 方法重载
        for (int i = 0; i < times; i++)
            System.out.println("Woof!");
    }
}
```

## 基本数据类型

```java
int    i = 42;          // 32位整数
long   l = 42L;         // 64位整数
float  f = 3.14f;       // 32位浮点
double d = 3.14;        // 64位浮点（默认）
boolean b = true;
char   c = 'A';         // 单引号
String s = "hello";     // 双引号，String 是类不是基本类型
```

## 条件与循环

```java
// if-else
if (x > 0) {
    System.out.println("positive");
} else if (x < 0) {
    System.out.println("negative");
} else {
    System.out.println("zero");
}

// while
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// for
for (int j = 0; j < 5; j++) {
    System.out.println(j);
}
```

## 常见坑

- Java 文件名必须和 `public class` 名**完全一致**（区分大小写）
- `String` 用 `.equals()` 比较内容，`==` 比较的是引用地址
- 整数除法自动截断：`5 / 2 = 2`，不是 2.5
