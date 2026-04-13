---
title: Java 14 - 集合框架与泛型
date: 2017-01-12
tags: [Java]
categories:
  - 编程语言
  - Java
---

## 集合框架

```
Collection
├── List（有序，可重复）
│   ├── ArrayList（动态数组，查询快）
│   └── LinkedList（链表，插入删除快）
└── Set（无序，不可重复）
    ├── HashSet（哈希表）
    └── TreeSet（红黑树，有序）

Map（键值对）
├── HashMap（哈希表，无序）
└── TreeMap（红黑树，按key有序）
```

## List

```java
import java.util.*;

List<String> list = new ArrayList<>();
list.add("Banana");
list.add("Apple");
list.add("Cherry");
list.add(1, "Mango");          // 在索引1处插入

System.out.println(list.get(0));       // Banana
System.out.println(list.size());       // 4
System.out.println(list.indexOf("Apple")); // 2

list.remove("Apple");
list.remove(0);                // 按索引删除

Collections.sort(list);        // 排序
System.out.println(list);      // [Cherry, Mango]
```

## Set

```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Apple");   // 重复，不会添加

System.out.println(set.size());        // 2
System.out.println(set.contains("Apple")); // true

// TreeSet 自动排序
Set<Integer> treeSet = new TreeSet<>();
treeSet.add(5);
treeSet.add(1);
treeSet.add(3);
System.out.println(treeSet);   // [1, 3, 5]
```

## Map

```java
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 90);
map.put("Bob", 85);
map.put("Charlie", 92);

System.out.println(map.get("Alice"));      // 90
System.out.println(map.containsKey("Bob")); // true
System.out.println(map.size());            // 3

map.remove("Bob");

// 遍历
for (String key : map.keySet())
    System.out.println(key + ": " + map.get(key));

for (Map.Entry<String, Integer> entry : map.entrySet())
    System.out.println(entry.getKey() + ": " + entry.getValue());
```

## 泛型

```java
// 没有泛型（Java 5之前）
ArrayList list = new ArrayList();
list.add("hello");
list.add(42);
String s = (String) list.get(0);  // 需要强转，运行时可能ClassCastException

// 有泛型（Java 5+）
ArrayList<String> list2 = new ArrayList<String>();
list2.add("hello");
// list2.add(42);  // 编译错误，类型安全
String s2 = list2.get(0);  // 无需强转
```

## 泛型类和方法

```java
// 泛型类
public class Box<T> {
    private T content;

    public void set(T content) { this.content = content; }
    public T get() { return content; }
}

Box<String> strBox = new Box<>();
strBox.set("Hello");
String s = strBox.get();

Box<Integer> intBox = new Box<>();
intBox.set(42);
int n = intBox.get();

// 泛型方法
public <T extends Comparable<T>> T max(T a, T b) {
    return a.compareTo(b) > 0 ? a : b;
}
```

## Collections 工具类

```java
List<Integer> list = Arrays.asList(3, 1, 4, 1, 5, 9);

Collections.sort(list);          // 升序排序
Collections.reverse(list);       // 反转
Collections.shuffle(list);       // 随机打乱
Collections.max(list);           // 最大值
Collections.min(list);           // 最小值
Collections.frequency(list, 1);  // 元素出现次数
```

## 常见坑

- `ArrayList` 按索引查询 O(1)，`LinkedList` 按索引查询 O(n)
- `HashMap` 无序，`TreeMap` 按 key 排序，`LinkedHashMap` 保持插入顺序
- `HashSet` 依赖 `hashCode()` 和 `equals()`，自定义类放入 Set 要覆盖这两个方法
- 遍历时不能用 `list.remove()`，要用 `Iterator.remove()` 或 `removeIf()`
