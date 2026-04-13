---
title: CSS 02 - 盒模型
date: 2017-06-02
tags: [CSS, 前端]
categories:
  - 全栈开发
  - 前端
---

## 盒模型结构

```
┌─────────────────────────────┐
│           margin            │
│  ┌───────────────────────┐  │
│  │        border         │  │
│  │  ┌─────────────────┐  │  │
│  │  │     padding     │  │  │
│  │  │  ┌───────────┐  │  │  │
│  │  │  │  content  │  │  │  │
│  │  │  └───────────┘  │  │  │
│  │  └─────────────────┘  │  │
│  └───────────────────────┘  │
└─────────────────────────────┘
```

```css
div {
    width: 200px;
    height: 100px;
    padding: 20px;           /* 内边距 */
    border: 2px solid #333;  /* 边框 */
    margin: 10px;            /* 外边距 */
}
/* 标准盒模型总宽度 = 200 + 20*2 + 2*2 = 244px */
```

## box-sizing

```css
/* 标准盒模型（默认）：width = content 宽度 */
box-sizing: content-box;

/* IE盒模型：width = content + padding + border */
box-sizing: border-box;  /* 推荐，更直观 */

/* 全局设置 */
* {
    box-sizing: border-box;
}
```

## padding / margin 简写

```css
/* 四个值：上 右 下 左（顺时针）*/
padding: 10px 20px 10px 20px;

/* 两个值：上下 左右 */
padding: 10px 20px;

/* 一个值：四边相同 */
padding: 10px;

/* 单独设置 */
padding-top: 10px;
padding-right: 20px;
padding-bottom: 10px;
padding-left: 20px;
```

## border

```css
border: 1px solid #333;
border: 2px dashed red;
border: 3px dotted blue;

/* 单边 */
border-top: 1px solid #ccc;
border-bottom: none;

/* 圆角 */
border-radius: 5px;
border-radius: 50%;    /* 圆形（width=height时）*/
border-radius: 10px 20px 10px 20px;
```

## margin 特殊行为

```css
/* 外边距合并：相邻块级元素的上下margin取较大值 */
.box1 { margin-bottom: 20px; }
.box2 { margin-top: 30px; }
/* 实际间距是 30px，不是 50px */

/* 水平居中 */
.center {
    width: 800px;
    margin: 0 auto;
}

/* margin: auto 只对块级元素且有固定宽度有效 */
```

## 块级 vs 行内元素

```css
/* 块级元素：独占一行，可设宽高 */
/* div, p, h1~h6, ul, li, table */

/* 行内元素：不换行，宽高由内容决定，上下 padding/margin 无效 */
/* span, a, strong, em, img */

/* 行内块：不换行，但可设宽高 */
/* img, input, button */

/* 修改显示方式 */
span { display: block; }
div  { display: inline; }
span { display: inline-block; }
div  { display: none; }   /* 隐藏且不占位 */
div  { visibility: hidden; }  /* 隐藏但占位 */
```

## 常见坑

- 使用 `border-box` 可以避免计算总宽度时出错，建议全局设置
- 外边距合并只发生在**块级元素的上下 margin**，左右不合并
- `margin: auto` 实现水平居中必须给元素设置固定宽度
- 行内元素设置上下 `margin` / `padding` 不会影响布局（视觉上有效果但不占空间）
