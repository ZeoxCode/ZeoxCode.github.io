---
title: 数据库 03 - SQL基础（DDL + DML）
date: 2017-02-25
tags: [数据库, SQL]
categories:
  - 计算机基础
  - 数据库
---

## DDL 数据定义

### 建表

```sql
CREATE TABLE Student (
    Sno    CHAR(9)     PRIMARY KEY,
    Sname  VARCHAR(20) NOT NULL,
    Ssex   CHAR(2),
    Sage   INT,
    Sdept  VARCHAR(20)
);

-- 带外键
CREATE TABLE SC (
    Sno   CHAR(9),
    Cno   CHAR(4),
    Grade INT,
    PRIMARY KEY (Sno, Cno),
    FOREIGN KEY (Sno) REFERENCES Student(Sno),
    FOREIGN KEY (Cno) REFERENCES Course(Cno)
);
```

### 修改表结构

```sql
ALTER TABLE Student ADD Email VARCHAR(50);       -- 加列
ALTER TABLE Student DROP COLUMN Email;           -- 删列
ALTER TABLE Student MODIFY Sage SMALLINT;        -- 改列类型
```

### 删除表

```sql
DROP TABLE Student;          -- 删表（含数据）
TRUNCATE TABLE Student;      -- 清空数据，保留结构
```

## DML 数据操作

### 插入

```sql
INSERT INTO Student VALUES ('201501', '张三', '男', 20, 'CS');
INSERT INTO Student (Sno, Sname) VALUES ('201502', '李四');  -- 部分列
```

### 修改

```sql
UPDATE Student SET Sage = 21 WHERE Sno = '201501';
UPDATE SC SET Grade = Grade * 1.1 WHERE Cno = 'C001';  -- 批量更新
```

### 删除

```sql
DELETE FROM Student WHERE Sno = '201501';
DELETE FROM SC WHERE Sno NOT IN (SELECT Sno FROM Student);
```

## 常见坑

- `DROP` 删表结构+数据，`TRUNCATE` 只删数据，`DELETE` 可加条件逐行删
- 插入时列数和值数必须一一对应
- 更新/删除不加 `WHERE` 会影响整张表，后果不可逆
