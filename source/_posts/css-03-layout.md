---
title: CSS 03 - 浮动与定位
date: 2017-06-10
tags: [CSS, 前端]
categories:
  - 全栈开发
  - 前端
---

## 浮动

```css
/* 浮动：元素脱离文档流，向左/右靠拢 */
.left  { float: left; }
.right { float: right; }
.none  { float: none; }
```

```html
<!-- 两列布局 -->
<div class="container">
    <div class="sidebar" style="float:left; width:200px;">侧边栏</div>
    <div class="main" style="float:left; width:600px;">主内容</div>
</div>
```

### 清除浮动

浮动元素脱离文档流，父容器高度塌陷，需要清除浮动：

```css
/* 方法1：clearfix（推荐）*/
.clearfix::after {
    content: "";
    display: block;
    clear: both;
}

/* 方法2：overflow */
.container {
    overflow: hidden;  /* 或 overflow: auto */
}

/* 方法3：clear 属性 */
.clear { clear: both; }
```

## 定位

```css
position: static;    /* 默认，正常文档流 */
position: relative;  /* 相对自身原位置偏移，仍占空间 */
position: absolute;  /* 相对最近的非static祖先定位，脱离文档流 */
position: fixed;     /* 相对视口定位，滚动不动 */
position: sticky;    /* 滚动到一定位置后固定 */
```

```css
/* 配合 top / right / bottom / left 使用 */
.box {
    position: absolute;
    top: 20px;
    right: 30px;
}
```

### relative

```css
.box {
    position: relative;
    top: 20px;    /* 向下移动 20px（相对原位置）*/
    left: 10px;   /* 向右移动 10px */
}
/* 原来的位置仍然占着，只是视觉上偏移了 */
```

### absolute

```css
/* 父元素需要设置 position: relative 作为定位参考 */
.parent {
    position: relative;
    width: 400px;
    height: 300px;
}

.child {
    position: absolute;
    bottom: 10px;
    right: 10px;
}

/* 绝对居中 */
.center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

### fixed

```css
/* 固定在视口，常用于导航栏、回到顶部按钮 */
.navbar {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 100;   /* 层叠顺序，数值越大越在上层 */
}

.back-to-top {
    position: fixed;
    bottom: 30px;
    right: 30px;
}
```

## z-index

```css
/* 只对非 static 定位元素有效 */
.modal   { position: fixed; z-index: 1000; }
.overlay { position: fixed; z-index: 999; }
.navbar  { position: fixed; z-index: 100; }
```

## 常见坑

- `absolute` 定位参考的是最近**非 static** 的祖先，如果都是 static 则参考 body
- 浮动元素**宽度由内容决定**，最好显式设置宽度
- `z-index` 只在同一**层叠上下文**内比较，跨层叠上下文比较无效
- `fixed` 定位在移动端有坑，建议配合 `transform` 使用
