---
title: Java 06 - Java 类库与 ArrayList
date: 2016-11-08
tags: [Java]
categories:
  - 编程语言
  - Java
---

## ArrayList

数组长度固定，ArrayList 长度动态可变。

```java
import java.util.ArrayList;

ArrayList<String> list = new ArrayList<String>();

list.add("Fido");
list.add("Rex");
list.add("Spot");

System.out.println(list.size());       // 3
System.out.println(list.get(0));       // Fido
System.out.println(list.contains("Rex")); // true

list.remove("Rex");
list.remove(0);                        // 按索引删除

// 遍历
for (String s : list)
    System.out.println(s);
```

## ArrayList vs 数组

```java
// 数组：固定大小，基本类型
int[] arr = new int[5];
arr[0] = 1;

// ArrayList：动态大小，只能存对象
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(1);   // 自动装箱 int → Integer
int n = list.get(0);  // 自动拆箱 Integer → int
```

## String 常用方法

```java
String s = "Hello, World!";

s.length()              // 13
s.charAt(0)             // 'H'
s.indexOf("World")      // 7
s.substring(7)          // "World!"
s.substring(7, 12)      // "World"
s.toUpperCase()         // "HELLO, WORLD!"
s.toLowerCase()         // "hello, world!"
s.trim()                // 去除首尾空白
s.replace("World", "Java")  // "Hello, Java!"
s.startsWith("Hello")  // true
s.endsWith("!")         // true
s.split(", ")           // ["Hello", "World!"]
s.equals("hello")       // false（区分大小写）
s.equalsIgnoreCase("hello, world!")  // true
```

## StringBuilder

频繁字符串拼接用 StringBuilder，比 `+` 效率高：

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(", ");
sb.append("World");
sb.append("!");
String result = sb.toString();  // "Hello, World!"

// 链式调用
String s = new StringBuilder()
    .append("a")
    .append("b")
    .append("c")
    .toString();  // "abc"
```

## 查 API 文档

Java 5 API 文档包含所有类库说明：
- `java.lang`：String, Math, Integer, System（自动导入）
- `java.util`：ArrayList, Arrays, Collections, Random
- `java.io`：File, InputStream, OutputStream

## 常见坑

- `ArrayList` 不能存基本类型，只能存对象（用包装类 Integer, Double 等）
- `String` 是不可变的，每次操作都返回新字符串，原字符串不变
- `==` 比较字符串引用，`.equals()` 比较内容，一定用 `.equals()`
