---
title: HTML 01 - 基本结构与常用标签
date: 2017-05-05
tags: [HTML, 前端]
categories:
  - 全栈开发
  - 前端
---

## 基本结构

```html
<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>页面标题</title>
</head>
<body>
    <!-- 页面内容 -->
</body>
</html>
```

## 文本标签

```html
<h1>一级标题</h1>
<h2>二级标题</h2>
<h6>六级标题</h6>

<p>这是一个段落。</p>

<strong>加粗</strong>
<em>斜体</em>
<u>下划线</u>
<del>删除线</del>

<br>  <!-- 换行，自闭合标签 -->
<hr>  <!-- 水平线 -->
```

## 链接与图片

```html
<!-- 链接 -->
<a href="https://example.com">外部链接</a>
<a href="about.html">内部链接</a>
<a href="#section1">锚点链接</a>
<a href="https://example.com" target="_blank">新标签页打开</a>

<!-- 图片 -->
<img src="photo.jpg" alt="图片描述" width="300" height="200">
```

## 列表

```html
<!-- 无序列表 -->
<ul>
    <li>苹果</li>
    <li>香蕉</li>
    <li>橙子</li>
</ul>

<!-- 有序列表 -->
<ol>
    <li>第一步</li>
    <li>第二步</li>
    <li>第三步</li>
</ol>

<!-- 定义列表 -->
<dl>
    <dt>HTML</dt>
    <dd>超文本标记语言</dd>
    <dt>CSS</dt>
    <dd>层叠样式表</dd>
</dl>
```

## div 与 span

```html
<!-- div：块级容器，独占一行 -->
<div class="container">
    <div class="header">头部</div>
    <div class="content">内容</div>
</div>

<!-- span：行内容器，不换行 -->
<p>这是<span style="color:red">红色</span>文字</p>
```

## 常见坑

- `<!DOCTYPE html>` 必须在第一行，否则浏览器进入怪异模式
- `<img>` `<br>` `<hr>` `<input>` 是自闭合标签，不需要结束标签
- `alt` 属性在图片加载失败时显示，对可访问性很重要
- `id` 全页面唯一，`class` 可以多个元素共用
