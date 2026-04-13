---
title: JS 03 - DOM 操作
date: 2017-07-20
tags: [JavaScript, 前端]
categories:
  - 全栈开发
  - 前端
---

## 获取元素

```javascript
// 单个元素
document.getElementById("myId")
document.querySelector(".myClass")       // CSS选择器，返回第一个
document.querySelector("#id .class > p")

// 多个元素（返回 NodeList）
document.querySelectorAll(".item")
document.getElementsByClassName("item")
document.getElementsByTagName("p")

// 遍历 NodeList
const items = document.querySelectorAll(".item");
items.forEach(item => console.log(item));
// 或
for (let i = 0; i < items.length; i++)
    console.log(items[i]);
```

## 修改内容

```javascript
const el = document.querySelector("#box");

// 文本内容
el.textContent = "Hello";        // 纯文本，安全
el.innerHTML = "<b>Hello</b>";   // 可插入HTML（小心XSS）

// 属性
el.getAttribute("src")
el.setAttribute("src", "new.jpg")
el.removeAttribute("disabled")
el.id
el.className
el.href

// dataset（data-* 属性）
// <div data-user-id="123">
el.dataset.userId   // "123"
```

## 修改样式

```javascript
const el = document.querySelector(".box");

// 行内样式
el.style.color = "red";
el.style.fontSize = "20px";       // 驼峰命名
el.style.backgroundColor = "blue";
el.style.display = "none";

// class 操作（推荐）
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");    // 有则删，无则加
el.classList.contains("active");  // 判断是否有
el.classList.replace("old", "new");
```

## 创建与删除元素

```javascript
// 创建
const div = document.createElement("div");
div.textContent = "新元素";
div.className = "item";

// 插入
document.body.appendChild(div)           // 末尾插入
document.body.prepend(div)              // 开头插入
parent.insertBefore(div, referenceEl)   // 在指定元素前插入
referenceEl.after(div)                  // 在指定元素后插入

// 删除
el.remove()
parent.removeChild(el)

// 克隆
const clone = el.cloneNode(true)  // true=深克隆（含子元素）
```

## 遍历 DOM

```javascript
el.parentElement          // 父元素
el.children               // 子元素集合（HTMLCollection）
el.firstElementChild      // 第一个子元素
el.lastElementChild       // 最后一个子元素
el.nextElementSibling     // 下一个兄弟元素
el.previousElementSibling // 上一个兄弟元素
```

## 常见坑

- `innerHTML` 会解析 HTML，用户输入内容不要直接插入（XSS风险），用 `textContent`
- `querySelectorAll` 返回静态 NodeList，`getElementsByClassName` 返回动态 HTMLCollection
- JS 在 `<head>` 中执行时 DOM 还未加载，要放 `<body>` 末尾或用 `DOMContentLoaded` 事件
- `classList.toggle` 可以传第二个参数强制添加/删除：`el.classList.toggle("active", true)`
