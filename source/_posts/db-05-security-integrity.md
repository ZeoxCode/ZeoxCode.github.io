---
title: 数据库 05 - 安全性与完整性
date: 2017-03-12
tags: [数据库, SQL]
categories:
  - 计算机基础
  - 数据库
---

## 安全性

### 权限控制

```sql
-- 授权
GRANT SELECT, INSERT ON Student TO user1;
GRANT ALL PRIVILEGES ON Student TO user1 WITH GRANT OPTION;  -- 可转授

-- 撤销
REVOKE INSERT ON Student FROM user1;
```

### 视图控制

```sql
-- 用视图限制用户只能看部分数据
CREATE VIEW CS_Student AS
SELECT * FROM Student WHERE Sdept = 'CS';

GRANT SELECT ON CS_Student TO user1;
-- user1 只能看CS系学生，看不到其他系
```

## 完整性

### 约束类型

```sql
CREATE TABLE Student (
    Sno   CHAR(9)  PRIMARY KEY,                    -- 主键约束
    Sname VARCHAR(20) NOT NULL,                    -- 非空约束
    Ssex  CHAR(2)  CHECK (Ssex IN ('男', '女')),   -- CHECK约束
    Sage  INT      CHECK (Sage BETWEEN 15 AND 50),
    Sdept VARCHAR(20) DEFAULT 'CS'                 -- 默认值
);
```

### 参照完整性违约处理

```sql
FOREIGN KEY (Sno) REFERENCES Student(Sno)
    ON DELETE CASCADE    -- 级联删除
    ON UPDATE CASCADE;   -- 级联更新

-- 或者
    ON DELETE SET NULL   -- 删除时外键置NULL
    ON DELETE NO ACTION  -- 拒绝删除（默认）
```

## 常见坑

- `WITH GRANT OPTION` 允许被授权者继续授权给其他人
- `REVOKE` 收回权限时，被转授出去的权限**也会级联收回**
- CHECK 约束在 MySQL 5.x 中**不生效**，需要用触发器实现（MySQL 8.0 起才支持）
