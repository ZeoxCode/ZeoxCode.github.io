---
title: C/C++学习总结（十六）——构造函数与析构函数
date: 2016-10-23
categories:
  - 全栈开发
  - 后端
tags:
  - C++
---

## 构造函数

对象创建时自动调用，函数名与类名相同，无返回值：

```cpp
class Student {
public:
    int id;
    string name;

    Student() {           // 默认构造函数
        id = 0;
        name = "";
    }

    Student(int i, string n) {  // 有参构造函数
        id = i;
        name = n;
    }
};

Student s1;             // 调用默认构造
Student s2(1001, "张三");  // 调用有参构造
```

## 初始化列表

```cpp
Student(int i, string n) : id(i), name(n) {}
```

const 成员变量和引用成员只能用初始化列表赋值。

## 析构函数

对象销毁时自动调用，用于释放资源：

```cpp
~Student() {
    // 清理工作，如free内存
}
```

---

**问题：** 有参构造函数定义后，默认构造函数消失
```cpp
class A {
    A(int x) { ... }
};
A obj;  // 编译错误，没有默认构造函数
```
**解决：** 显式定义默认构造函数 `A() {}`，或使用 `A() = default;`（C++11）

---

**问题：** 类含有指针成员，未在析构函数中释放，内存泄漏
```cpp
class A {
    int *data;
public:
    A() { data = new int[100]; }
    // 没有析构函数
};
```
**解决：**
```cpp
~A() { delete[] data; }
```
