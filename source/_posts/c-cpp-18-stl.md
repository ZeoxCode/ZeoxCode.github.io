---
title: C/C++学习总结（十八）——STL初步
date: 2016-10-31
categories:
  - 编程语言
  - C/C++
tags:
  - C++
---

## vector

动态数组，自动扩容：

```cpp
#include <vector>
vector<int> v;

v.push_back(10);
v.push_back(20);
v.push_back(30);

cout << v[0] << endl;   // 10
cout << v.size() << endl;  // 3

v.pop_back();  // 删除末尾
```

## string

```cpp
#include <string>
string s = "hello";
s += " world";
cout << s.length() << endl;    // 11
cout << s.substr(0, 5) << endl; // hello
cout << s.find("world") << endl; // 6
```

## map

键值对，按 key 自动排序：

```cpp
#include <map>
map<string, int> m;
m["apple"] = 3;
m["banana"] = 5;

cout << m["apple"] << endl;  // 3
cout << m.count("apple") << endl;  // 1（存在）
```

## 迭代器

```cpp
for (auto it = v.begin(); it != v.end(); it++) {
    cout << *it << endl;
}

// C++11 range-for
for (int x : v) {
    cout << x << endl;
}
```

---

**问题：** 用下标访问 map 中不存在的 key，会自动插入默认值
```cpp
map<string, int> m;
cout << m["nokey"] << endl;  // 输出0，同时插入了 "nokey":0
```
**解决：** 先用 `count` 或 `find` 判断是否存在
```cpp
if (m.count("nokey")) { cout << m["nokey"]; }
```

---

**问题：** vector 遍历时删除元素导致迭代器失效
```cpp
for (auto it = v.begin(); it != v.end(); it++) {
    if (*it == 2) v.erase(it);  // 迭代器失效，未定义行为
}
```
**解决：**
```cpp
it = v.erase(it);  // erase返回下一个有效迭代器
```
