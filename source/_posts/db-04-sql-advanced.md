---
title: 数据库 04 - SQL进阶（连接、子查询、聚合）
date: 2017-03-05
tags: [数据库, SQL]
categories:
  - 计算机基础
  - 数据库
---

## 查询基本结构

```sql
SELECT [DISTINCT] 列名
FROM 表名
[WHERE 条件]
[GROUP BY 列名]
[HAVING 分组条件]
[ORDER BY 列名 ASC|DESC];
```

执行顺序：`FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY`

## 连接查询

```sql
-- 内连接（等值连接）
SELECT S.Sname, C.Cname, SC.Grade
FROM Student S
JOIN SC ON S.Sno = SC.Sno
JOIN Course C ON SC.Cno = C.Cno;

-- 左外连接（保留左表所有行）
SELECT S.Sname, SC.Grade
FROM Student S
LEFT JOIN SC ON S.Sno = SC.Sno;
```

## 子查询

```sql
-- IN 子查询
SELECT Sname FROM Student
WHERE Sno IN (
    SELECT Sno FROM SC WHERE Cno = 'C001'
);

-- EXISTS 子查询
SELECT Sname FROM Student S
WHERE EXISTS (
    SELECT 1 FROM SC WHERE Sno = S.Sno AND Cno = 'C001'
);

-- 比较子查询
SELECT Sname FROM Student
WHERE Sage > (SELECT AVG(Sage) FROM Student);
```

## 聚合函数

```sql
SELECT
    COUNT(*)        AS 总人数,
    AVG(Grade)      AS 平均分,
    MAX(Grade)      AS 最高分,
    MIN(Grade)      AS 最低分,
    SUM(Grade)      AS 总分
FROM SC
WHERE Cno = 'C001';

-- 分组统计
SELECT Cno, AVG(Grade)
FROM SC
GROUP BY Cno
HAVING AVG(Grade) >= 70;  -- 分组后筛选用 HAVING
```

## 常见坑

- `WHERE` 不能用聚合函数，聚合函数的条件要放 `HAVING`
- `IN` 和 `EXISTS` 结果相同，大表用 `EXISTS` 效率更高
- `COUNT(*)` 计所有行，`COUNT(列名)` 不计 NULL
- `DISTINCT` 去重，`SELECT DISTINCT Sno FROM SC` 只返回不重复的学号

## 经典例题

**查询选了全部课程的学生姓名（用双重否定）：**

```sql
SELECT Sname FROM Student S
WHERE NOT EXISTS (
    SELECT * FROM Course C
    WHERE NOT EXISTS (
        SELECT * FROM SC
        WHERE Sno = S.Sno AND Cno = C.Cno
    )
);
-- 含义：不存在一门课，该学生没选
```
