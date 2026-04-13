---
title: Java 05 - 方法进阶与数组操作
date: 2016-11-01
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 增强 for 循环

```java
int[] nums = {1, 2, 3, 4, 5};

// 传统 for
for (int i = 0; i < nums.length; i++)
    System.out.println(nums[i]);

// 增强 for（for-each）
for (int n : nums)
    System.out.println(n);

// 字符串数组
String[] names = {"Alice", "Bob", "Charlie"};
for (String name : names)
    System.out.println(name);
```

## 类型转换进阶

```java
// 自动提升：运算时小类型自动提升为大类型
byte b = 10;
byte c = 20;
// byte d = b + c;  // 错误！b+c 结果是 int
int d = b + c;      // 正确

// 强制转换
long l = 1234567890123L;
int i = (int) l;    // 数据丢失！
```

## 数组工具

```java
import java.util.Arrays;

int[] arr = {5, 3, 1, 4, 2};

Arrays.sort(arr);
System.out.println(Arrays.toString(arr));  // [1, 2, 3, 4, 5]

int idx = Arrays.binarySearch(arr, 3);    // 有序数组二分查找
System.out.println(idx);                  // 2

int[] copy = Arrays.copyOf(arr, arr.length);  // 复制数组
```

## Math 类

```java
Math.abs(-5)        // 5
Math.max(3, 7)      // 7
Math.min(3, 7)      // 3
Math.round(3.7)     // 4（四舍五入）
Math.floor(3.9)     // 3.0（向下取整）
Math.ceil(3.1)      // 4.0（向上取整）
Math.pow(2, 10)     // 1024.0
Math.sqrt(16)       // 4.0
Math.random()       // [0.0, 1.0) 随机数

// 生成 0~99 随机整数
int rand = (int)(Math.random() * 100);
```

## 方法中的数组操作示例

```java
// 找数组最大值
int max(int[] arr) {
    int max = arr[0];
    for (int n : arr)
        if (n > max) max = n;
    return max;
}

// 数组求和
int sum(int[] arr) {
    int total = 0;
    for (int n : arr)
        total += n;
    return total;
}

// 判断是否包含某值
boolean contains(int[] arr, int val) {
    for (int n : arr)
        if (n == val) return true;
    return false;
}
```

## 常见坑

- `Math.random()` 返回 `double`，强转 `int` 时要先乘再转
- 数组是对象，传入方法后方法内修改会影响原数组
- `Arrays.sort()` 是原地排序，直接修改原数组
