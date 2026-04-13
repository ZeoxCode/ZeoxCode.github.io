---
title: CSS 05 - 过渡与动画
date: 2017-06-26
tags: [CSS, 前端]
categories:
  - 全栈开发
  - 前端
---

## 过渡（transition）

状态变化时的平滑动画效果：

```css
.button {
    background: blue;
    color: white;
    padding: 10px 20px;
    transition: background 0.3s ease;
    /* 属性名 持续时间 缓动函数 延迟时间 */
}

.button:hover {
    background: darkblue;
}

/* 多个属性 */
.box {
    transition: width 0.3s ease, opacity 0.5s linear;
}

/* 所有属性 */
.box {
    transition: all 0.3s ease;
}
```

## 缓动函数

```css
transition-timing-function: ease;        /* 慢-快-慢（默认）*/
transition-timing-function: linear;      /* 匀速 */
transition-timing-function: ease-in;     /* 慢-快 */
transition-timing-function: ease-out;    /* 快-慢 */
transition-timing-function: ease-in-out; /* 慢-快-慢 */
```

## 变换（transform）

```css
.box {
    /* 位移 */
    transform: translateX(50px);
    transform: translateY(-20px);
    transform: translate(50px, -20px);

    /* 旋转 */
    transform: rotate(45deg);
    transform: rotate(-90deg);

    /* 缩放 */
    transform: scale(1.5);        /* 等比放大1.5倍 */
    transform: scale(2, 0.5);     /* 宽2倍，高0.5倍 */

    /* 倾斜 */
    transform: skew(10deg, 5deg);

    /* 组合（从右到左执行）*/
    transform: translate(50px, 0) rotate(45deg) scale(1.2);

    /* 变换基点（默认 center center）*/
    transform-origin: top left;
}
```

## 动画（animation）

```css
/* 定义关键帧 */
@keyframes fadeIn {
    from { opacity: 0; }
    to   { opacity: 1; }
}

@keyframes bounce {
    0%   { transform: translateY(0); }
    50%  { transform: translateY(-30px); }
    100% { transform: translateY(0); }
}

/* 应用动画 */
.box {
    animation: fadeIn 1s ease;
    /* 名称 持续时间 缓动 延迟 次数 方向 填充模式 */

    animation-name: bounce;
    animation-duration: 0.5s;
    animation-timing-function: ease;
    animation-delay: 0.2s;
    animation-iteration-count: infinite;  /* 无限循环 */
    animation-direction: alternate;       /* 来回播放 */
    animation-fill-mode: forwards;        /* 保持最终状态 */
}

/* 暂停动画 */
.box:hover {
    animation-play-state: paused;
}
```

## 常用动画示例

```css
/* 旋转加载 */
@keyframes spin {
    from { transform: rotate(0deg); }
    to   { transform: rotate(360deg); }
}
.loader {
    width: 40px;
    height: 40px;
    border: 4px solid #f3f3f3;
    border-top: 4px solid #3498db;
    border-radius: 50%;
    animation: spin 1s linear infinite;
}

/* 淡入上移 */
@keyframes fadeInUp {
    from {
        opacity: 0;
        transform: translateY(30px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```

## 常见坑

- `transition` 需要有**起始和结束状态**才能触发，动画是主动播放
- `transform` 不会影响文档流，元素位置不变，只是视觉上移动
- `transform: translate(-50%, -50%)` 配合绝对定位实现精确居中
- 尽量用 `transform` 和 `opacity` 做动画，这两个属性由 GPU 加速，性能最好
