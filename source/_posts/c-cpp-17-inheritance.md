---
title: C/C++学习总结（十七）——继承与多态
date: 2016-10-27
categories:
  - 编程语言
  - C/C++
tags:
  - C++
---

## 继承

```cpp
class Animal {
public:
    string name;
    void eat() { cout << "eating" << endl; }
};

class Dog : public Animal {
public:
    void bark() { cout << "woof" << endl; }
};

Dog d;
d.name = "旺财";
d.eat();   // 继承自Animal
d.bark();  // Dog自己的
```

## 虚函数与多态

```cpp
class Animal {
public:
    virtual void speak() { cout << "..." << endl; }
};

class Dog : public Animal {
public:
    void speak() override { cout << "woof" << endl; }
};

class Cat : public Animal {
public:
    void speak() override { cout << "meow" << endl; }
};

Animal *a = new Dog();
a->speak();  // 输出 woof，而不是 ...
```

不加 `virtual`，`a->speak()` 调用的是 Animal 的版本。

## 纯虚函数 / 抽象类

```cpp
class Shape {
public:
    virtual double area() = 0;  // 纯虚函数，子类必须实现
};
```

含纯虚函数的类不能实例化。

---

**问题：** 基类析构函数不是 virtual，delete 基类指针时子类析构未调用，内存泄漏
```cpp
Animal *a = new Dog();
delete a;  // 如果~Animal()不是virtual，~Dog()不会被调用
```
**解决：** 基类析构函数声明为 virtual
```cpp
virtual ~Animal() {}
```

---

**问题：** 子类 override 拼写与基类不一致，没有真正覆盖
```cpp
class Dog : public Animal {
    void Speak() { ... }  // 大写S，不是override，是新函数
};
```
**解决：** C++11 起加 `override` 关键字，拼写错误时编译报错
```cpp
void speak() override { ... }
```
