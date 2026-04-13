---
title: Python 05 - 造自己的数据类型
date: 2017-09-11
tags: [Python]
categories:
  - 编程语言
  - Python
---

## 定义类

```python
class Dog:
    # 类属性（所有实例共享）
    species = "Canis familiaris"

    # __init__ 是构造方法
    def __init__(self, name, age):
        self.name = name   # 实例属性
        self.age = age

    def bark(self):
        print(f"{self.name} says: Woof!")

    def __str__(self):
        return f"Dog({self.name}, {self.age})"


d = Dog("Rex", 3)
d.bark()       # Rex says: Woof!
print(d)       # Dog(Rex, 3)
```

## 继承

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError("子类必须实现这个方法")

class Dog(Animal):
    def speak(self):
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

animals = [Dog("Rex"), Cat("Tom")]
for a in animals:
    print(a.speak())   # 多态
```

## 调用父类方法

```python
class GuideDog(Dog):
    def __init__(self, name, owner):
        super().__init__(name)    # 调用父类 __init__
        self.owner = owner

    def speak(self):
        base = super().speak()    # 调用父类 speak
        return base + " (guide dog)"
```

## 常用魔术方法

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):
        return f"Vector({self.x!r}, {self.y!r})"

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __len__(self):
        return 2

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y


v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)   # Vector(4, 6)
print(len(v1))   # 2
print(v1 == v1)  # True
```

## @property

把方法伪装成属性访问。

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("半径不能为负")
        self._radius = value

    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2


c = Circle(5)
print(c.area)    # 78.53...
c.radius = 10    # 调用 setter
```

## 常见坑

- 忘写 `self` 是最常见的错误，Python 不会自动传入
- `__str__` 是给 `print()` 用的，`__repr__` 是给开发者看的（调试用）
- Python 没有真正的 `private`，`_name` 是约定俗成"不要从外部访问"，`__name` 会触发名称修饰但也可以访问
- 类属性被实例修改后，只影响该实例，不影响其他实例和类本身
