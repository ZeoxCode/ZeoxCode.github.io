---
title: CSS 01 - 选择器与基本样式
date: 2017-05-25
tags: [CSS, 前端]
categories:
  - 全栈开发
  - 前端
---

## 引入方式

```html
<!-- 行内样式（优先级最高，不推荐） -->
<p style="color: red;">文字</p>

<!-- 内部样式 -->
<head>
    <style>
        p { color: red; }
    </style>
</head>

<!-- 外部样式（推荐） -->
<link rel="stylesheet" href="style.css">
```

## 选择器

```css
/* 元素选择器 */
p { color: red; }

/* 类选择器 */
.title { font-size: 24px; }

/* ID 选择器 */
#header { background: blue; }

/* 后代选择器（空格） */
div p { color: green; }        /* div 内所有 p */

/* 子元素选择器（>） */
div > p { color: green; }      /* div 直接子 p */

/* 相邻兄弟（+） */
h1 + p { margin-top: 0; }     /* h1 后紧跟的 p */

/* 群组选择器（,） */
h1, h2, h3 { font-family: sans-serif; }

/* 属性选择器 */
input[type="text"] { border: 1px solid #ccc; }
a[href^="https"] { color: green; }   /* href 以 https 开头 */
a[href$=".pdf"] { color: red; }      /* href 以 .pdf 结尾 */

/* 伪类 */
a:hover { color: orange; }           /* 鼠标悬停 */
a:visited { color: purple; }         /* 已访问链接 */
li:first-child { font-weight: bold; }
li:last-child { border: none; }
li:nth-child(2) { color: red; }      /* 第2个 */
li:nth-child(odd) { background: #f5f5f5; } /* 奇数行 */

/* 伪元素 */
p::first-line { font-weight: bold; }
p::before { content: "» "; }
p::after  { content: " «"; }
```

## 优先级

```
!important > 行内样式 > ID > 类/伪类/属性 > 元素/伪元素

计算方式（a, b, c）：
a = ID选择器数量
b = 类、伪类、属性选择器数量
c = 元素、伪元素选择器数量

#nav .item a  →  (1, 1, 1) = 0111
.item a       →  (0, 1, 1) = 0011
a             →  (0, 0, 1) = 0001
```

## 常用文本样式

```css
p {
    color: #333;                  /* 颜色 */
    font-size: 16px;              /* 字号 */
    font-family: Arial, sans-serif;
    font-weight: bold;            /* normal / bold / 100~900 */
    font-style: italic;
    line-height: 1.6;             /* 行高，推荐用倍数 */
    text-align: center;           /* left / right / center / justify */
    text-decoration: underline;   /* none / underline / line-through */
    text-transform: uppercase;    /* lowercase / capitalize */
    letter-spacing: 2px;          /* 字间距 */
    word-spacing: 4px;            /* 词间距 */
    text-indent: 2em;             /* 首行缩进 */
    white-space: nowrap;          /* 不换行 */
    overflow: hidden;
    text-overflow: ellipsis;      /* 超出显示省略号 */
}
```

## 常见坑

- ID 选择器优先级远高于类选择器，尽量少用 ID 选择器做样式
- 优先级相同时，**后写的覆盖先写的**
- `!important` 慎用，会破坏正常优先级，调试困难
- 伪元素 `::before` / `::after` 必须设置 `content` 属性才会显示
