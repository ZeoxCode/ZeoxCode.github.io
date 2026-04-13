---
title: 架构师备考 12 - 设计模式（创建型）
date: 2022-09-20
tags: [软考, 系统架构设计师, 设计模式]
categories:
  - 软考备考
  - 系统架构设计师
---

## 设计模式概述

**GoF 23种设计模式**分三类：
- **创建型（5种）**：对象创建机制，解耦对象的创建和使用
- **结构型（7种）**：类和对象的组合方式
- **行为型（11种）**：对象之间的职责分配和通信

**六大设计原则**：
- **单一职责**：一个类只有一个引起它变化的原因
- **开闭原则**：对扩展开放，对修改关闭
- **里氏替换**：子类必须能替换父类
- **接口隔离**：不应强迫客户依赖不需要的接口
- **依赖倒置**：高层模块不依赖低层模块，都依赖抽象
- **迪米特法则**：只与直接朋友通信（最少知识原则）

## 工厂方法（Factory Method）

**意图**：定义创建对象的接口，让子类决定实例化哪个类。

```
Creator（抽象工厂）
    + factoryMethod(): Product  ← 抽象方法
    + anOperation()

ConcreteCreatorA               ConcreteCreatorB
    + factoryMethod(): ProductA    + factoryMethod(): ProductB
```

- 将对象创建延迟到子类
- 符合开闭原则：增加新产品只需增加新的具体工厂，不修改已有代码
- 典型场景：日志框架（FileLogger/DBLogger）、UI组件（WindowsButton/MacButton）

## 抽象工厂（Abstract Factory）

**意图**：提供创建**一系列相关产品**（产品族）的接口，而不指定具体类。

```
AbstractFactory
    + createProductA(): AbstractProductA
    + createProductB(): AbstractProductB

ConcreteFactory1（Windows风格）    ConcreteFactory2（Mac风格）
    + createProductA(): WinButtonA     + createProductA(): MacButtonA
    + createProductB(): WinMenuB       + createProductB(): MacMenuB
```

- 工厂方法针对**一个**产品等级，抽象工厂针对**多个**产品等级（一个产品族）
- 优点：保证产品族的一致性
- 缺点：新增产品种类（纵向扩展）需要修改所有工厂，违反开闭原则

## 单例（Singleton）

**意图**：确保类只有一个实例，并提供全局访问点。

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}   // 私有构造器

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**双重检查锁（线程安全，高效）**：
```java
private volatile static Singleton instance;

public static Singleton getInstance() {
    if (instance == null) {
        synchronized (Singleton.class) {
            if (instance == null) {
                instance = new Singleton();
            }
        }
    }
    return instance;
}
```

- 需要 `volatile` 防止指令重排序
- 典型场景：配置管理器、线程池、日志对象

## 建造者（Builder）

**意图**：将复杂对象的构建过程与表示分离，同样的构建过程可以创建不同的表示。

```
Director（指挥者）
    + construct(Builder)

Builder（抽象）              ConcreteBuilderA
    + buildPartA()               + buildPartA()
    + buildPartB()               + buildPartB()
    + getResult(): Product       + getResult(): ProductA
```

- 适合：对象构建步骤固定，但每步的具体实现不同
- 与工厂方法区别：Builder 关注**分步骤构建复杂对象**，工厂方法关注**一步创建对象**
- 典型场景：SQL QueryBuilder、HTML生成器、配置对象构建

## 原型（Prototype）

**意图**：通过复制（克隆）已有对象来创建新对象，而不是通过构造函数。

```java
public class ConcretePrototype implements Cloneable {
    private String data;

    @Override
    public ConcretePrototype clone() {
        try {
            return (ConcretePrototype) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

**浅克隆 vs 深克隆**：
- 浅克隆：基本类型复制值，引用类型复制引用（共享对象）
- 深克隆：完全复制，包括引用的对象（完全独立）

- 适合：对象创建代价大，可以通过复制已有对象来提高效率
- 典型场景：游戏中复制怪物、图形编辑器中复制图形

## 创建型模式对比

| 模式 | 解决的问题 | 关键词 |
|------|----------|--------|
| 工厂方法 | 创建哪个子类的对象 | 一个产品，子类决定 |
| 抽象工厂 | 创建一组相关产品 | 产品族，保证兼容 |
| 单例 | 全局唯一实例 | 唯一，全局访问 |
| 建造者 | 复杂对象的分步骤构建 | 分步构建，指挥者 |
| 原型 | 通过克隆创建对象 | 复制，Clone |
