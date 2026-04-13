---
title: Java 09 - 对象的生命周期与构造方法
date: 2016-11-29
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 构造方法

```java
public class Duck {
    String name;
    int size;

    // 默认构造方法（无参）
    public Duck() {
        name = "Unknown";
        size = 0;
    }

    // 有参构造方法
    public Duck(String name, int size) {
        this.name = name;
        this.size = size;
    }

    // 构造方法重载
    public Duck(String name) {
        this(name, 10);  // 调用另一个构造方法
    }
}
```

```java
Duck d1 = new Duck();               // 调用无参构造
Duck d2 = new Duck("Donald", 30);   // 调用有参构造
Duck d3 = new Duck("Daffy");        // 调用单参构造
```

## 继承中的构造方法

```java
public class Animal {
    String name;

    public Animal(String name) {
        this.name = name;
    }
}

public class Dog extends Animal {
    String breed;

    public Dog(String name, String breed) {
        super(name);         // 必须第一行调用父类构造方法
        this.breed = breed;
    }
}
```

如果父类没有无参构造，子类构造方法**必须**显式调用 `super(...)`。

## 栈与堆

```java
void go() {
    int x = 10;         // x 在栈上
    Dog d = new Dog();  // d（引用）在栈上，Dog对象在堆上
}
// go() 执行完毕，栈帧弹出，x 和 d 消失
// 但堆上的 Dog 对象还在，等待 GC 回收
```

```
栈（Stack）                堆（Heap）
┌──────────┐              ┌──────────────────┐
│ go()     │              │ Dog对象           │
│  x = 10  │              │  name = null      │
│  d ──────┼─────────────→│  size = 0         │
└──────────┘              └──────────────────┘
```

## 垃圾回收（GC）

```java
Dog d = new Dog();   // 创建对象
d = new Dog();       // 原来的 Dog 没有引用指向它了 → 可被 GC 回收
d = null;            // 手动置 null，对象可被 GC 回收
```

Java 自动 GC，不需要手动 `free()`（和 C/C++ 不同）。

## 变量初始值

```java
// 实例变量（成员变量）有默认值
int i;          // 0
double d;       // 0.0
boolean b;      // false
String s;       // null
Object obj;     // null

// 局部变量（方法内）没有默认值，使用前必须赋值
void go() {
    int x;
    // System.out.println(x);  // 编译错误：variable x might not have been initialized
}
```

## 常见坑

- 如果定义了有参构造方法，编译器**不再自动提供**无参构造方法
- `super()` 调用必须是构造方法的**第一条语句**
- `this()` 和 `super()` 不能同时出现在同一个构造方法中
- 局部变量必须显式初始化，成员变量有默认值
