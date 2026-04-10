---
title: C/C++学习总结（十三）——文件操作
date: 2016-10-14
categories:
  - 全栈开发
  - 后端
tags:
  - C
---

## fopen 模式

| 模式 | 说明 |
|------|------|
| `"r"` | 只读，文件不存在则失败 |
| `"w"` | 只写，不存在则创建，存在则清空 |
| `"a"` | 追加，不存在则创建 |
| `"rb"` / `"wb"` | 二进制读/写 |

## 文本读写

```c
FILE *fp = fopen("test.txt", "w");
fprintf(fp, "hello %d\n", 42);
fclose(fp);

FILE *fp = fopen("test.txt", "r");
char line[256];
while (fgets(line, sizeof(line), fp) != NULL) {
    printf("%s", line);
}
fclose(fp);
```

## 二进制读写

```c
// 写
fwrite(&s, sizeof(Student), 1, fp);
// 读
fread(&s, sizeof(Student), 1, fp);
```

---

**问题：** fopen 失败未判断，对 NULL 指针操作崩溃
```c
FILE *fp = fopen("nofile.txt", "r");
fprintf(fp, "hello");  // fp是NULL，段错误
```
**解决：**
```c
FILE *fp = fopen("test.txt", "r");
if (fp == NULL) { return -1; }
```

---

**问题：** 用 `"w"` 打开已有文件，原内容被清空
**解决：** 追加内容用 `"a"`，不要用 `"w"`。

---

**问题：** 忘记 fclose，缓冲区数据未写入磁盘
**解决：** 文件操作完毕必须调用 `fclose(fp)`。
