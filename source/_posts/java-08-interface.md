---
title: Java 08 - 接口与抽象类
date: 2016-11-22
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 抽象类

```java
// 抽象类不能实例化，必须被继承
public abstract class Shape {
    String color;

    // 抽象方法：没有方法体，子类必须实现
    public abstract double area();

    // 普通方法：子类可以直接用
    public void printColor() {
        System.out.println("Color: " + color);
    }
}

public class Circle extends Shape {
    double radius;

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    double width, height;

    @Override
    public double area() {
        return width * height;
    }
}
```

```java
// Shape s = new Shape();  // 编译错误，不能实例化抽象类
Shape s = new Circle();    // 多态：父类引用指向子类对象
```

## 接口

```java
// 接口：纯抽象，定义"能做什么"
public interface Flyable {
    void fly();               // 默认 public abstract
    int MAX_HEIGHT = 10000;   // 默认 public static final
}

public interface Swimmable {
    void swim();
}

// 一个类可以实现多个接口（解决Java单继承限制）
public class Duck extends Animal implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

## 接口作为类型

```java
Flyable[] flyers = new Flyable[3];
flyers[0] = new Duck();
flyers[1] = new Airplane();   // Airplane 也实现了 Flyable
flyers[2] = new Bird();

for (Flyable f : flyers)
    f.fly();
```

## 抽象类 vs 接口

| | 抽象类 | 接口 |
|--|--------|------|
| 实例化 | 不能 | 不能 |
| 方法实现 | 可以有 | 不能有（Java 5）|
| 变量 | 普通变量 | 只能是常量 |
| 继承/实现 | 单继承 | 可实现多个 |
| 用途 | IS-A 关系 | HAS-A 能力 |

## 经典例题

```java
// Comparable 接口：让对象可比较
public class Student implements Comparable<Student> {
    String name;
    double gpa;

    @Override
    public int compareTo(Student other) {
        return Double.compare(this.gpa, other.gpa);
        // 负数：this < other
        // 0：相等
        // 正数：this > other
    }
}
```

## 常见坑

- 接口中的变量默认是 `public static final`，不能被修改
- 抽象类可以有构造方法（供子类 super() 调用），但不能直接 new
- 实现接口必须实现**所有**抽象方法，否则该类也必须声明为 abstract
