---
title: HTML 02 - 表单与表格
date: 2017-05-12
tags: [HTML, 前端]
categories:
  - 全栈开发
  - 前端
---

## 表格

```html
<table border="1">
    <thead>
        <tr>
            <th>姓名</th>
            <th>年龄</th>
            <th>城市</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>张三</td>
            <td>20</td>
            <td>北京</td>
        </tr>
        <tr>
            <td>李四</td>
            <td>22</td>
            <td>上海</td>
        </tr>
    </tbody>
</table>
```

### 合并单元格

```html
<!-- colspan：横向合并 -->
<td colspan="2">跨两列</td>

<!-- rowspan：纵向合并 -->
<td rowspan="2">跨两行</td>
```

## 表单

```html
<form action="/submit" method="POST">

    <!-- 文本输入 -->
    <label for="username">用户名：</label>
    <input type="text" id="username" name="username" placeholder="请输入用户名">

    <!-- 密码 -->
    <input type="password" name="password" placeholder="请输入密码">

    <!-- 邮箱 -->
    <input type="email" name="email">

    <!-- 数字 -->
    <input type="number" name="age" min="0" max="120">

    <!-- 单选框 -->
    <input type="radio" name="gender" value="male" id="male">
    <label for="male">男</label>
    <input type="radio" name="gender" value="female" id="female">
    <label for="female">女</label>

    <!-- 复选框 -->
    <input type="checkbox" name="hobby" value="reading"> 阅读
    <input type="checkbox" name="hobby" value="coding"> 编程

    <!-- 下拉选择 -->
    <select name="city">
        <option value="">请选择</option>
        <option value="beijing">北京</option>
        <option value="shanghai" selected>上海</option>
    </select>

    <!-- 多行文本 -->
    <textarea name="bio" rows="4" cols="30" placeholder="个人简介"></textarea>

    <!-- 文件上传 -->
    <input type="file" name="avatar">

    <!-- 隐藏字段 -->
    <input type="hidden" name="token" value="abc123">

    <!-- 提交按钮 -->
    <input type="submit" value="提交">
    <button type="submit">提交</button>
    <button type="reset">重置</button>

</form>
```

## GET vs POST

| | GET | POST |
|--|-----|------|
| 数据位置 | URL参数 | 请求体 |
| 长度限制 | 有（约2KB）| 无 |
| 安全性 | 低（可见）| 较高 |
| 用途 | 查询 | 提交/修改 |

## 常见坑

- 同组单选框 `name` 必须相同，才能互斥
- `label` 的 `for` 要和 `input` 的 `id` 对应，点击标签可以聚焦输入框
- `method` 默认是 GET，提交敏感数据要用 POST
- `<th>` 是表头单元格，默认加粗居中；`<td>` 是普通单元格
