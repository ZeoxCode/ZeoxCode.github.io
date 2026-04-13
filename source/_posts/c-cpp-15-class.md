---
title: C/C++学习总结（十五）——类与对象
date: 2016-10-20
categories:
  - 编程语言
  - C/C++
tags:
  - C++
---

## 类的定义

```cpp
class Student {
public:
    int id;
    string name;
    float score;

    void print() {
        cout << id << " " << name << " " << score << endl;
    }
};

Student s;
s.id = 1001;
s.name = "张三";
s.score = 89.5;
s.print();
```

## 访问控制

- `public`：外部可访问
- `private`：仅类内部可访问（默认）
- `protected`：类内部和子类可访问

## this 指针

指向当前对象的指针，成员函数内隐式可用：

```cpp
void setId(int id) {
    this->id = id;  // 区分成员变量和参数同名
}
```

---

**问题：** 成员变量与参数同名，赋值无效
```cpp
void setId(int id) {
    id = id;  // 赋给自己，成员变量未改变
}
```
**解决：** 用 `this->id = id`，或改变参数名。

---

**问题：** 类外访问 private 成员，编译报错
```cpp
Student s;
s.score = 100;  // 如果score是private，编译错误
```
**解决：** 提供 public 的 getter / setter 方法。
```cpp
void setScore(float s) { score = s; }
float getScore() { return score; }
```
