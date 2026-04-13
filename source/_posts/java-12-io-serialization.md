---
title: Java 12 - 序列化与 IO
date: 2016-12-22
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 文件读写

### 读文本文件

```java
import java.io.*;

// 方式1：BufferedReader（推荐）
try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 方式2：逐字符读
FileReader fr = new FileReader("test.txt");
int c;
while ((c = fr.read()) != -1)
    System.out.print((char) c);
fr.close();
```

### 写文本文件

```java
try (BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"))) {
    bw.write("Hello, World!");
    bw.newLine();
    bw.write("Line 2");
} catch (IOException e) {
    e.printStackTrace();
}

// 追加写入
new FileWriter("out.txt", true);  // 第二个参数 true = append
```

## 序列化

将对象保存到文件（对象 → 字节流）：

```java
import java.io.*;

public class Dog implements Serializable {  // 必须实现 Serializable
    String name;
    int size;
    transient String secret;  // transient：不参与序列化
}

// 序列化（写入文件）
Dog d = new Dog();
d.name = "Fido";
d.size = 30;

try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("dog.ser"))) {
    oos.writeObject(d);
}

// 反序列化（从文件读取）
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("dog.ser"))) {
    Dog restored = (Dog) ois.readObject();
    System.out.println(restored.name);  // Fido
}
```

## IO 流体系

```
字节流（处理二进制）：
InputStream  → FileInputStream, BufferedInputStream, ObjectInputStream
OutputStream → FileOutputStream, BufferedOutputStream, ObjectOutputStream

字符流（处理文本）：
Reader → FileReader, BufferedReader
Writer → FileWriter, BufferedWriter
```

包装流（装饰器模式）：
```java
new BufferedReader(new FileReader("file.txt"))
// BufferedReader 包装 FileReader，提供缓冲和 readLine()
```

## File 类

```java
File f = new File("test.txt");

f.exists()          // 是否存在
f.isFile()          // 是否是文件
f.isDirectory()     // 是否是目录
f.length()          // 文件大小（字节）
f.getName()         // 文件名
f.getAbsolutePath() // 绝对路径
f.mkdir()           // 创建目录
f.delete()          // 删除

File dir = new File("myDir");
String[] files = dir.list();  // 列出目录下所有文件名
```

## 常见坑

- 流用完必须关闭，推荐用 `try-with-resources`（Java 7+），自动关闭
- 序列化的类需要定义 `serialVersionUID`，否则类修改后反序列化会失败
- `transient` 字段序列化后为默认值（null/0/false），不是原来的值
- 路径分隔符：Windows 用 `\`，Linux/Mac 用 `/`，用 `File.separator` 兼容
