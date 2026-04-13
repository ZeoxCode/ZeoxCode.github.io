---
title: 架构师备考 13 - 设计模式（结构型）
date: 2022-09-21
tags: [软考, 系统架构设计师, 设计模式]
categories:
  - 软考备考
  - 系统架构设计师
---

## 适配器（Adapter）

**意图**：将一个类的接口转换成客户期望的另一个接口，使原本不兼容的类可以合作。

```
Client → Target接口 ← Adapter → Adaptee（被适配者）
```

- **类适配器**：通过继承 Adaptee 实现
- **对象适配器**：持有 Adaptee 的引用（更灵活，推荐）
- 典型场景：整合第三方库、旧系统接口兼容、读卡器（内存卡→电脑）
- **关键词**：接口不兼容、转换、包装

## 桥接（Bridge）

**意图**：将抽象部分与实现部分分离，使它们可以独立变化。

```
Abstraction（抽象）──────────── Implementor（实现接口）
    + operation()                   + operationImpl()
         |                                |
RefinedAbstraction          ConcreteImplementorA / B
```

- 解决类爆炸问题：如果形状（圆/方）× 颜色（红/蓝）用继承会有4个类，桥接只需 2+2=4个
- 典型场景：跨平台UI（形状×绘图API）、设备驱动（逻辑设备×物理设备）
- **关键词**：两个维度独立变化，抽象和实现分离

## 组合（Composite）

**意图**：将对象组合成树形结构，使客户端对单个对象和组合对象的使用方式一致。

```
Component（抽象）
    + operation()
    + add(Component)
    + remove(Component)
    + getChild(int)

Leaf（叶节点）          Composite（容器节点）
    + operation()           + children: List<Component>
                            + operation()  // 遍历children
```

- 典型场景：文件系统（文件/文件夹）、菜单（菜单项/子菜单）、公司组织架构
- **关键词**：树形结构、叶节点和容器节点统一处理

## 装饰器（Decorator）

**意图**：动态地给对象添加额外职责，比继承更灵活。

```
Component（接口）
    + operation()

ConcreteComponent        Decorator（持有Component引用）
    + operation()            + component: Component
                             + operation() { component.operation() }

ConcreteDecoratorA       ConcreteDecoratorB
    + operation()            + operation()
    // 先调父类operation，再添加额外行为
```

- 装饰器和被装饰对象实现同一接口，可以嵌套多层
- 典型场景：Java I/O流（FileInputStream→BufferedInputStream→DataInputStream）、日志装饰、权限检查
- **关键词**：动态添加功能，不改变接口，可叠加

## 外观（Facade）

**意图**：为子系统中的一组接口提供一个统一的高层接口，使子系统更容易使用。

```
Client → Facade → SubSystemA
              ↘ SubSystemB
              ↘ SubSystemC
```

- 不是封装子系统，只是提供便捷入口，子系统仍可直接访问
- 典型场景：启动电脑（按一个键完成自检+加载BIOS+启动OS）、SDK封装、微服务API网关
- **关键词**：简化接口、统一入口、降低耦合

## 享元（Flyweight）

**意图**：通过共享大量细粒度对象，节省内存。

```
FlyweightFactory（工厂，维护享元池）
    + getFlyweight(key): Flyweight   // 有则返回，无则创建

ConcreteFlyweight
    + intrinsicState（内部状态，共享）
    + operation(extrinsicState)    // 外部状态由调用者传入
```

- **内部状态**：不变的，可以共享（如字符的字体、大小）
- **外部状态**：变化的，每次调用传入（如字符在文档中的位置）
- 典型场景：文字编辑器（26个字母共享，位置是外部状态）、游戏中大量相同粒子
- **关键词**：大量相似对象，共享内部状态，节省内存

## 代理（Proxy）

**意图**：为另一个对象提供一个替代品或占位符，控制对这个对象的访问。

```
Client → Proxy（与RealSubject同接口）→ RealSubject
```

**常见类型**：
- **远程代理**：为远程对象提供本地代理（RPC Stub）
- **虚拟代理**：延迟创建开销大的对象（懒加载图片占位符）
- **保护代理**：控制访问权限
- **缓存代理**：缓存结果，避免重复计算
- **智能引用代理**：引用计数、资源管理

- **关键词**：控制访问、延迟创建、权限检查、透明代理

## 结构型模式对比

| 模式 | 关键意图 | 记忆关键词 |
|------|---------|-----------|
| 适配器 | 接口转换 | 不兼容→兼容，转换器 |
| 桥接 | 抽象与实现分离 | 两维度独立变化 |
| 组合 | 树形结构统一处理 | 整体-部分，树 |
| 装饰器 | 动态添加功能 | 叠加、透明包装 |
| 外观 | 简化接口 | 统一入口，简化 |
| 享元 | 共享节省内存 | 大量对象，内外状态 |
| 代理 | 控制访问 | 替代品，访问控制 |

**适配器 vs 装饰器 vs 代理**：
- 适配器：改变接口（让不兼容的接口工作）
- 装饰器：增强功能（接口不变，叠加行为）
- 代理：控制访问（接口不变，控制访问时机/权限）
