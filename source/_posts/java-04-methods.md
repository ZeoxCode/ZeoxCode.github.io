---
title: Java 04 - 对象的行为（方法与封装）
date: 2016-10-24
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 方法参数与返回值

```java
public class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    String greet(String name) {
        return "Hello, " + name;
    }

    // 无返回值
    void printResult(int result) {
        System.out.println("Result: " + result);
    }
}
```

## 值传递（Pass by Value）

Java 只有值传递：
- 基本类型：传值的副本，方法内修改不影响外部
- 引用类型：传引用的副本，方法内可通过引用修改对象，但重新赋值不影响外部

```java
void changeInt(int x) {
    x = 100;           // 不影响外部
}

void changeDog(Dog d) {
    d.name = "Max";    // 影响外部（同一个对象）
    d = new Dog();     // 不影响外部（只改了副本引用）
}
```

## 封装

```java
public class BankAccount {
    private double balance;  // private：外部不能直接访问

    public void deposit(double amount) {
        if (amount > 0)
            balance += amount;
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance)
            balance -= amount;
    }

    public double getBalance() {  // getter
        return balance;
    }
}
```

```java
BankAccount acc = new BankAccount();
// acc.balance = -100;  // 编译错误，private
acc.deposit(500);
System.out.println(acc.getBalance());  // 500.0
```

## getter / setter 规范

```java
public class Person {
    private String name;
    private int age;

    // getter
    public String getName() { return name; }
    public int getAge() { return age; }

    // setter
    public void setName(String name) {
        this.name = name;  // this 区分成员变量和参数
    }

    public void setAge(int age) {
        if (age > 0) this.age = age;  // 加验证逻辑
    }
}
```

## 方法重载

```java
public class Printer {
    void print(int i)    { System.out.println(i); }
    void print(String s) { System.out.println(s); }
    void print(double d) { System.out.println(d); }
    // 方法名相同，参数列表不同（类型/数量/顺序）
    // 返回值不同不算重载
}
```

## 常见坑

- `this` 关键字指当前对象，用于区分同名的成员变量和局部变量
- Java 只有值传递，没有引用传递（和 C++ 的引用不同）
- 重载时编译器根据参数类型选择方法，如果有歧义会编译错误
