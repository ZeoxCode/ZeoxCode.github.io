---
title: Java 10 - static、final 与包装类
date: 2016-12-07
tags: [Java]
categories:
  - 编程语言
  - Java
---

## static

```java
public class Counter {
    private static int count = 0;  // 类变量，所有实例共享
    private int id;

    public Counter() {
        count++;
        id = count;
    }

    public static int getCount() {  // 静态方法，无需实例即可调用
        return count;
    }
}
```

```java
Counter c1 = new Counter();
Counter c2 = new Counter();
Counter c3 = new Counter();

System.out.println(Counter.getCount());  // 3，用类名调用静态方法
System.out.println(c1.getCount());       // 3（不推荐，但合法）
```

## static 变量 vs 实例变量

```java
public class Dog {
    static String species = "Canis lupus";  // 所有 Dog 共享
    String name;                            // 每个 Dog 自己的
}

Dog d1 = new Dog();
Dog d2 = new Dog();
d1.name = "Fido";
d2.name = "Rex";
Dog.species = "Dog";  // 改了所有 Dog 的 species
```

## final

```java
// final 变量：常量，不能修改
public static final double PI = 3.14159;
final int MAX = 100;
// MAX = 200;  // 编译错误

// final 方法：不能被子类覆盖
public final void critical() { ... }

// final 类：不能被继承（String 就是 final 类）
public final class ImmutablePoint { ... }
```

## 包装类与自动装箱

```java
// 基本类型 → 包装类
Integer i = new Integer(42);   // Java 5 之前
Integer i2 = 42;               // 自动装箱（Java 5+）

// 包装类 → 基本类型
int n = i2;                    // 自动拆箱

// 常用工具方法
Integer.parseInt("42")         // String → int
Integer.toBinaryString(10)     // "1010"
Integer.toHexString(255)       // "ff"
Integer.MAX_VALUE              // 2147483647
Integer.MIN_VALUE              // -2147483648

Double.parseDouble("3.14")
Double.isNaN(0.0 / 0.0)       // true
```

## 枚举

```java
public enum Direction {
    NORTH, SOUTH, EAST, WEST
}

Direction d = Direction.NORTH;

switch (d) {
    case NORTH: System.out.println("Going north"); break;
    case SOUTH: System.out.println("Going south"); break;
}

// 遍历
for (Direction dir : Direction.values())
    System.out.println(dir);
```

## 常见坑

- 静态方法中不能用 `this`，也不能直接访问实例变量
- `Integer == Integer` 在 -128~127 范围内返回 true（缓存），超出范围要用 `.equals()`
- `final` 引用变量不能重新指向对象，但对象内容可以修改
- `static` 代码块在类加载时执行一次，早于构造方法
