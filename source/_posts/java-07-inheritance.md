---
title: Java 07 - 继承与多态
date: 2016-11-15
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 继承

```java
// 父类
public class Animal {
    String name;
    int hunger;

    void eat() {
        System.out.println(name + " is eating");
        hunger--;
    }

    void sleep() {
        System.out.println(name + " is sleeping");
    }

    void makeNoise() {
        System.out.println("...");
    }
}

// 子类
public class Dog extends Animal {
    void fetch() {
        System.out.println(name + " is fetching");
    }

    @Override
    void makeNoise() {
        System.out.println("Woof!");  // 覆盖父类方法
    }
}

public class Cat extends Animal {
    @Override
    void makeNoise() {
        System.out.println("Meow!");
    }
}
```

## 多态

```java
Animal[] animals = new Animal[3];
animals[0] = new Dog();   // Dog 是 Animal
animals[1] = new Cat();   // Cat 是 Animal
animals[2] = new Dog();

for (Animal a : animals) {
    a.makeNoise();  // 运行时决定调用哪个版本
}
// 输出：
// Woof!
// Meow!
// Woof!
```

## 类型判断与转换

```java
Animal a = new Dog();

// instanceof 判断类型
if (a instanceof Dog) {
    Dog d = (Dog) a;  // 向下转型
    d.fetch();
}

// 直接强转（不安全，可能抛 ClassCastException）
Dog d = (Dog) a;
```

## super 关键字

```java
public class Dog extends Animal {
    String breed;

    @Override
    void makeNoise() {
        super.makeNoise();        // 调用父类方法
        System.out.println("Woof!");
    }
}
```

## Object 类

所有类都隐式继承 `Object`，常用方法：

```java
Object obj = new Dog();

obj.getClass()        // 返回运行时类
obj.getClass().getName()  // "Dog"

// 覆盖 toString()
public class Dog extends Animal {
    @Override
    public String toString() {
        return "Dog: " + name;
    }
}
System.out.println(dog);  // 自动调用 toString()

// 覆盖 equals()
@Override
public boolean equals(Object obj) {
    if (!(obj instanceof Dog)) return false;
    Dog other = (Dog) obj;
    return this.name.equals(other.name);
}
```

## 常见坑

- `@Override` 注解会让编译器检查是否真的覆盖了父类方法，建议总是加上
- 子类不能继承父类的 `private` 成员，但可以通过 `public` 方法访问
- 向下转型前一定用 `instanceof` 判断，否则可能 `ClassCastException`
- Java 只支持单继承（一个类只能 `extends` 一个父类）
