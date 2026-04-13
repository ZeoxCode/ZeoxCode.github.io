---
title: HTML 03 - HTML5 语义化标签
date: 2017-05-18
tags: [HTML, 前端]
categories:
  - 全栈开发
  - 前端
---

## 语义化标签

HTML5 之前全用 `<div>`，HTML5 引入了有意义的标签：

```html
<body>
    <header>
        <nav>
            <a href="/">首页</a>
            <a href="/about">关于</a>
        </nav>
    </header>

    <main>
        <article>
            <h1>文章标题</h1>
            <section>
                <h2>第一节</h2>
                <p>内容...</p>
            </section>
            <section>
                <h2>第二节</h2>
                <p>内容...</p>
            </section>
        </article>

        <aside>
            <p>侧边栏：相关推荐</p>
        </aside>
    </main>

    <footer>
        <p>版权所有 © 2017</p>
    </footer>
</body>
```

## 各标签用途

| 标签 | 用途 |
|------|------|
| `<header>` | 页眉，网站顶部 |
| `<nav>` | 导航菜单 |
| `<main>` | 页面主体内容（每页只有一个）|
| `<article>` | 独立内容（博客文章、新闻）|
| `<section>` | 文章内的分节 |
| `<aside>` | 侧边栏、附加内容 |
| `<footer>` | 页脚 |
| `<figure>` | 图片+说明的组合 |
| `<figcaption>` | 图片说明文字 |

## 多媒体标签

```html
<!-- 视频 -->
<video width="640" height="360" controls>
    <source src="video.mp4" type="video/mp4">
    <source src="video.webm" type="video/webm">
    你的浏览器不支持 video 标签
</video>

<!-- 音频 -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    你的浏览器不支持 audio 标签
</audio>
```

## HTML5 新增 input 类型

```html
<input type="date">          <!-- 日期选择器 -->
<input type="time">          <!-- 时间选择器 -->
<input type="color">         <!-- 颜色选择器 -->
<input type="range" min="0" max="100">  <!-- 滑块 -->
<input type="search">        <!-- 搜索框（带清除按钮）-->
<input type="url">           <!-- URL输入（自动验证格式）-->
<input type="tel">           <!-- 电话号码（移动端弹出数字键盘）-->

<!-- required：必填 -->
<!-- pattern：正则验证 -->
<input type="text" required pattern="[0-9]{11}" placeholder="手机号">
```

## 常见坑

- 语义化标签和 `<div>` 在渲染上没有区别，区别在于语义和 SEO
- `<article>` 可以嵌套，`<section>` 是文章内的分节，不要混用
- `<main>` 每个页面只能有一个
- `controls` 属性让视频/音频显示播放控件，不加则不显示
