---
title: 数据库 06 - 关系数据理论与范式
date: 2017-03-20
tags: [数据库, SQL]
categories:
  - 计算机基础
  - 数据库
---

## 函数依赖

```
X → Y：X 函数决定 Y，即 X 的值能唯一确定 Y 的值

完全函数依赖：X → Y，且X的任何真子集都不能决定Y
部分函数依赖：X → Y，但X的某个真子集也能决定Y
传递函数依赖：X → Y，Y → Z，且Y不决定X，则X传递依赖Z
```

## 各级范式

### 1NF

所有属性都是**原子的**，不可再分。

```
违反1NF的例子：
联系方式字段里存 "138xxxx,010-xxxx"（多值）
```

### 2NF

在1NF基础上，**非主属性完全依赖于主键**（消除部分依赖）。

```
表：SC(Sno, Cno, Grade, Sname)
主键：(Sno, Cno)
问题：Sname 只依赖 Sno，部分依赖主键 → 违反2NF

拆分：
SC(Sno, Cno, Grade)
Student(Sno, Sname)
```

### 3NF

在2NF基础上，**非主属性不传递依赖于主键**（消除传递依赖）。

```
表：Student(Sno, Sdept, Dname)
Sno → Sdept → Dname（传递依赖）→ 违反3NF

拆分：
Student(Sno, Sdept)
Dept(Sdept, Dname)
```

### BCNF

所有决定属性的左部都是**候选键**（比3NF更严格）。

```
大多数情况下，达到3NF就够了
BCNF消除了主属性对候选键的部分/传递依赖
```

## 常见坑

- 2NF只对**复合主键**有意义，单属性主键自动满足2NF
- 规范化越高，冗余越少，但**查询时join越多**，性能下降
- 实际工程中为了性能，有时会**故意反范式化**保留冗余

## 经典例题

**判断 SLC(Sno, Sdept, Sloc, Cno, Grade) 属于第几范式：**

```
主键：(Sno, Cno)
函数依赖：
  Sno → Sdept
  Sdept → Sloc（传递：Sno → Sloc）
  (Sno, Cno) → Grade

Sdept、Sloc 部分依赖主键 → 不满足2NF → 属于1NF
```
