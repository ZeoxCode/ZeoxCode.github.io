---
title: Java 02 - 面向对象初探
date: 2016-10-10
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 类与对象

```java
// 定义类
public class Dog {
    String name;
    int size;

    void bark() {
        if (size > 60)
            System.out.println(name + ": Wooof!");
        else
            System.out.println(name + ": Yip!");
    }
}

// 创建对象（实例化）
Dog d = new Dog();   // new 在堆上分配内存
d.name = "Buddy";
d.size = 70;
d.bark();            // Buddy: Wooof!
```

## 对象引用

```java
Dog a = new Dog();
Dog b = a;           // b 和 a 指向同一个对象
b.name = "Max";
System.out.println(a.name);  // Max（同一个对象）

Dog c = new Dog();   // c 指向新对象
```

```
栈（Stack）          堆（Heap）
a ─────────────────→ [Dog: name="Max", size=0]
b ─────────────────↗
c ─────────────────→ [Dog: name=null, size=0]
```

## 一个简单的程序示例

```java
public class DogTestDrive {
    public static void main(String[] args) {
        Dog[] dogs = new Dog[3];

        dogs[0] = new Dog();
        dogs[0].name = "Fido";
        dogs[0].size = 30;

        dogs[1] = new Dog();
        dogs[1].name = "Rex";
        dogs[1].size = 80;

        dogs[2] = new Dog();
        dogs[2].name = "Spot";
        dogs[2].size = 55;

        for (int i = 0; i < dogs.length; i++)
            dogs[i].bark();
    }
}
```

## 常见坑

- 对象引用默认值是 `null`，调用 `null` 对象的方法会抛 `NullPointerException`
- 数组也是对象，`new Dog[3]` 创建了数组但没创建里面的 Dog 对象
- 对象在堆上，引用变量在栈上
