---
title: CSS 04 - Flexbox 布局
date: 2017-06-18
tags: [CSS, 前端]
categories:
  - 全栈开发
  - 前端
---

## 基本概念

```css
.container {
    display: flex;  /* 开启 flex 布局 */
}
/* 子元素自动变成 flex item，横向排列 */
```

```
主轴（main axis）：默认水平方向 →
交叉轴（cross axis）：默认垂直方向 ↓
```

## 容器属性

```css
.container {
    display: flex;

    /* 主轴方向 */
    flex-direction: row;            /* 默认：从左到右 */
    flex-direction: row-reverse;    /* 从右到左 */
    flex-direction: column;         /* 从上到下 */
    flex-direction: column-reverse; /* 从下到上 */

    /* 换行 */
    flex-wrap: nowrap;   /* 默认：不换行 */
    flex-wrap: wrap;     /* 超出换行 */

    /* 主轴对齐 */
    justify-content: flex-start;    /* 默认：左对齐 */
    justify-content: flex-end;      /* 右对齐 */
    justify-content: center;        /* 居中 */
    justify-content: space-between; /* 两端对齐，间隔均匀 */
    justify-content: space-around;  /* 每个元素两侧间隔相等 */

    /* 交叉轴对齐 */
    align-items: stretch;      /* 默认：拉伸填满 */
    align-items: flex-start;   /* 顶部对齐 */
    align-items: flex-end;     /* 底部对齐 */
    align-items: center;       /* 垂直居中 */

    /* 间距（现代浏览器支持）*/
    gap: 10px;
    gap: 10px 20px;   /* 行间距 列间距 */
}
```

## 子元素属性

```css
.item {
    /* 排列顺序（默认0，越小越靠前）*/
    order: 1;

    /* 放大比例（默认0，不放大）*/
    flex-grow: 1;

    /* 缩小比例（默认1，空间不足时缩小）*/
    flex-shrink: 0;  /* 0 = 不缩小 */

    /* 初始大小 */
    flex-basis: 200px;

    /* 简写：flex-grow flex-shrink flex-basis */
    flex: 1;           /* = flex: 1 1 0 */
    flex: 0 0 200px;   /* 固定200px，不放大不缩小 */

    /* 单独对齐（覆盖容器的 align-items）*/
    align-self: center;
}
```

## 常用布局

```css
/* 水平垂直居中 */
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

/* 导航栏：左侧 logo，右侧菜单 */
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

/* 等宽三列 */
.container { display: flex; }
.col { flex: 1; }

/* 左固定右自适应 */
.sidebar { flex: 0 0 200px; }
.main    { flex: 1; }

/* 底部固定的布局 */
.page {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}
.content { flex: 1; }   /* 撑满剩余空间 */
.footer  { }
```

## 常见坑

- `flex: 1` 是 `flex-grow: 1; flex-shrink: 1; flex-basis: 0` 的简写，不是 `flex-basis: auto`
- 设了 `display: flex` 后，子元素的 `float` 失效
- `align-items` 控制交叉轴（纵向），`justify-content` 控制主轴（横向），容易记混
- 父容器没设高度时，`align-items: center` 垂直居中不生效
