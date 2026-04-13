---
title: 架构师备考 14 - 设计模式（行为型）
date: 2022-09-22
tags: [软考, 系统架构设计师, 设计模式]
categories:
  - 系统架构
  - 系统架构设计师
---

## 观察者（Observer）

**意图**：定义对象间的一对多依赖，一个对象状态改变时，所有依赖者都收到通知并自动更新。

```
Subject（被观察者）
    + observers: List<Observer>
    + attach(Observer)
    + detach(Observer)
    + notify()   // 遍历通知所有观察者

Observer（接口）
    + update()

ConcreteObserver
    + update()   // 执行响应逻辑
```

- 也叫**发布-订阅**（但严格来说发布-订阅有中间件，观察者是直接通知）
- 典型场景：事件监听、MVC中Model通知View、股票行情推送
- **关键词**：一对多，松耦合，自动通知

## 策略（Strategy）

**意图**：定义一系列算法，封装每个算法，使它们可以互相替换。

```
Context（上下文）
    + strategy: Strategy
    + setStrategy(Strategy)
    + executeStrategy()

Strategy（接口）          ConcreteStrategyA / B / C
    + execute()               + execute()
```

- 消除大量 if-else/switch
- 典型场景：排序算法切换、支付方式（支付宝/微信/银行卡）、促销策略
- **关键词**：算法族，运行时切换，消除条件分支

## 模板方法（Template Method）

**意图**：在父类中定义算法的骨架，将某些步骤的实现延迟到子类。

```java
abstract class Game {
    // 模板方法（final，不可重写）
    public final void play() {
        initialize();   // 固定步骤
        startPlay();    // 抽象方法，子类实现
        endPlay();      // 抽象方法，子类实现
    }

    abstract void startPlay();
    abstract void endPlay();

    void initialize() { System.out.println("游戏初始化"); }
}
```

- 典型场景：数据处理流程（读取→处理→输出）、JUnit测试生命周期
- **关键词**：骨架固定，步骤可变，继承复用

## 责任链（Chain of Responsibility）

**意图**：将请求的发送者和接收者解耦，让多个对象都有机会处理请求，沿着链传递直到被处理。

```
Handler（抽象）
    + nextHandler: Handler
    + setNext(Handler)
    + handle(request)   // 能处理则处理，否则传给next

ConcreteHandlerA → ConcreteHandlerB → ConcreteHandlerC
```

- 请求可能被链中任意一个处理器处理，也可能都不处理
- 典型场景：审批流程（员工→经理→总监→CEO）、过滤器链（Servlet Filter）、异常处理
- **关键词**：链式传递，解耦发送者接收者

## 命令（Command）

**意图**：将请求封装成对象，支持撤销、队列、日志等操作。

```
Invoker（调用者）→ Command（接口）→ Receiver（接收者）
                     + execute()
                     + undo()

ConcreteCommand
    + receiver: Receiver
    + execute() { receiver.action() }
    + undo()
```

- 典型场景：文本编辑器的撤销/重做、任务队列、宏命令（命令组合）
- **关键词**：请求对象化，支持撤销，解耦调用者和执行者

## 迭代器（Iterator）

**意图**：提供顺序访问集合元素的方式，而不暴露集合的内部结构。

```java
interface Iterator<T> {
    boolean hasNext();
    T next();
}
```

- Java 的 `for-each` 循环就是基于迭代器
- 典型场景：遍历各种数据结构（数组、链表、树）用统一接口
- **关键词**：顺序访问，隐藏内部结构

## 状态（State）

**意图**：允许对象在内部状态改变时改变其行为，看起来像改变了类。

```
Context
    + state: State
    + setState(State)
    + request() { state.handle(this) }

State（接口）           ConcreteStateA / B
    + handle(Context)       + handle(Context)  // 可以切换Context的state
```

- 消除大量状态判断的 if-else
- 和策略模式很像，区别：状态模式的状态切换是自动的（在处理过程中切换），策略是外部主动设置
- 典型场景：订单状态机（待付款→已付款→已发货→已收货）、自动售货机、TCP连接状态
- **关键词**：状态机，行为随状态变化

## 中介者（Mediator）

**意图**：用一个中介对象封装一系列对象之间的交互，降低对象间的耦合。

```
Colleague（同事）← Mediator（中介） → Colleague
同事之间不直接通信，都通过中介者转发
```

- 典型场景：聊天室（用户不直接通信，消息通过服务器转发）、机场塔台（飞机不直接通讯，通过塔台调度）
- **关键词**：去除网状依赖，通过中介通信

## 备忘录（Memento）

**意图**：在不破坏封装的前提下，捕获并保存对象的内部状态，以便之后恢复。

```
Originator（原发器）→ Memento（备忘录，保存状态）← Caretaker（负责人，保管备忘录）
```

- 典型场景：撤销操作（游戏存档、文本编辑器历史）
- **关键词**：快照，存档，撤销恢复

## 访问者（Visitor）

**意图**：在不改变数据结构的前提下，定义作用于元素的新操作。

```
Element（接口）
    + accept(Visitor)

ConcreteElement
    + accept(Visitor v) { v.visit(this) }

Visitor（接口）
    + visit(ConcreteElementA)
    + visit(ConcreteElementB)
```

- 典型场景：编译器的语法树遍历（不同操作：求值、代码生成、类型检查）
- **关键词**：数据结构稳定，操作经常变化，双分派

## 解释器（Interpreter）

**意图**：给定一个语言，定义文法的表示，并定义一个解释器来解释该语言的句子。

- 典型场景：正则表达式解析、SQL解析、计算器
- 一般不直接实现，用 ANTLR 等工具生成

## 行为型模式对比

| 模式 | 关键词 | 解决什么 |
|------|--------|---------|
| 观察者 | 一对多通知 | 对象间依赖通知 |
| 策略 | 算法族切换 | 消除条件分支 |
| 模板方法 | 骨架+钩子 | 复用算法结构 |
| 责任链 | 链式传递 | 解耦请求与处理 |
| 命令 | 请求对象化 | 撤销/队列/日志 |
| 状态 | 状态机 | 行为随状态变化 |
| 中介者 | 中央协调 | 减少网状依赖 |
| 备忘录 | 快照存档 | 撤销恢复 |
| 迭代器 | 统一遍历 | 隐藏集合结构 |
| 访问者 | 双分派 | 操作与数据结构分离 |
| 解释器 | 文法/语言 | 语法解析 |
