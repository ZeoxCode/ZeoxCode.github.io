---
title: Java 03 - 变量与类型
date: 2016-10-17
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 基本类型 vs 引用类型

```
基本类型（存值本身）：
byte    8位   -128 ~ 127
short   16位  -32768 ~ 32767
int     32位  -2^31 ~ 2^31-1
long    64位  后缀L
float   32位  后缀f，精度约7位
double  64位  默认浮点类型
boolean 1位   true/false
char    16位  Unicode字符，单引号

引用类型（存对象地址）：
String, Dog, int[], ...
```

## 基本类型在栈上

```java
int x = 42;     // 栈上直接存 42
int y = x;      // 复制值，y 改变不影响 x
y = 100;
System.out.println(x);  // 42
```

## 引用类型在堆上

```java
Dog a = new Dog();
a.name = "Fido";
Dog b = a;          // 复制引用（地址），两者指向同一对象
b.name = "Rex";
System.out.println(a.name);  // Rex
```

## 类型转换

```java
// 隐式转换（小→大，安全）
int i = 100;
long l = i;         // int → long 自动转换
double d = i;       // int → double 自动转换

// 显式转换（大→小，可能丢精度）
double pi = 3.14;
int n = (int) pi;   // n = 3，小数截断

// 字符串转数字
int x = Integer.parseInt("42");
double y = Double.parseDouble("3.14");

// 数字转字符串
String s = String.valueOf(42);
String s2 = 42 + "";   // 简便写法
```

## 数组

```java
// 声明并初始化
int[] nums = new int[5];        // 默认值全0
int[] nums2 = {1, 2, 3, 4, 5}; // 直接赋值

String[] names = new String[3]; // 默认值全null

// 访问
nums[0] = 10;
System.out.println(nums.length); // 5（属性，不是方法）

// 二维数组
int[][] matrix = new int[3][4];
matrix[0][0] = 1;
```

## 常见坑

- `long` 字面量必须加 `L`：`long x = 3000000000L`，不加会编译错误
- `float` 字面量必须加 `f`：`float f = 3.14f`
- 数组下标从 0 开始，访问 `arr[arr.length]` 会抛 `ArrayIndexOutOfBoundsException`
- 基本类型不能为 `null`，引用类型可以
