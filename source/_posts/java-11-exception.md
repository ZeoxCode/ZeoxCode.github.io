---
title: Java 11 - 异常处理
date: 2016-12-15
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 异常层次

```
Throwable
├── Error（JVM级别错误，不要捕获）
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception
    ├── RuntimeException（非受检异常，可不处理）
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── ClassCastException
    │   └── NumberFormatException
    └── 受检异常（必须处理）
        ├── IOException
        └── SQLException
```

## try-catch-finally

```java
try {
    int[] arr = new int[5];
    arr[10] = 1;               // 抛出 ArrayIndexOutOfBoundsException
    System.out.println("不会执行到这");
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("数组越界：" + e.getMessage());
} catch (Exception e) {
    System.out.println("其他异常：" + e.getMessage());
} finally {
    System.out.println("finally 总会执行");  // 无论是否异常都执行
}
```

## 多个 catch

```java
try {
    String s = null;
    s.length();
} catch (NullPointerException e) {
    System.out.println("空指针");
} catch (Exception e) {
    System.out.println("其他");
}
// 顺序：子类在前，父类在后
```

## throws 声明

```java
// 受检异常必须声明或捕获
public void readFile(String path) throws IOException {
    FileReader fr = new FileReader(path);
    // ...
}

// 调用者必须处理
try {
    readFile("test.txt");
} catch (IOException e) {
    e.printStackTrace();
}
```

## 自定义异常

```java
public class InsufficientFundsException extends Exception {
    private double amount;

    public InsufficientFundsException(double amount) {
        super("余额不足，缺少：" + amount);
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}

// 使用
public void withdraw(double amount) throws InsufficientFundsException {
    if (amount > balance)
        throw new InsufficientFundsException(amount - balance);
    balance -= amount;
}
```

## 常见异常场景

```java
// NullPointerException
String s = null;
s.length();  // NPE

// ArrayIndexOutOfBoundsException
int[] arr = new int[5];
arr[5] = 1;  // AIOOBE

// NumberFormatException
int n = Integer.parseInt("abc");  // NFE

// ClassCastException
Object obj = "hello";
Integer i = (Integer) obj;  // CCE

// StackOverflowError
void infinite() { infinite(); }  // 无限递归
```

## 常见坑

- `finally` 块中的 `return` 会覆盖 `try`/`catch` 中的 `return`，避免在 `finally` 中写 `return`
- 捕获异常顺序：**子类在前，父类在后**，否则编译错误
- `RuntimeException` 不需要声明，但受检异常必须 `throws` 或 `try-catch`
- `e.printStackTrace()` 打印完整堆栈，调试时很有用
