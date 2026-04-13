---
title: JS 04 - 事件处理
date: 2017-07-28
tags: [JavaScript, 前端]
categories:
  - 全栈开发
  - 前端
---

## 绑定事件

```javascript
const btn = document.querySelector("#btn");

// 方式1：addEventListener（推荐）
btn.addEventListener("click", function(e) {
    console.log("点击了", e.target);
});

// 方式2：on 属性（会覆盖）
btn.onclick = function(e) {
    console.log("点击");
};

// 移除事件（需要具名函数）
function handleClick(e) { console.log("点击"); }
btn.addEventListener("click", handleClick);
btn.removeEventListener("click", handleClick);
```

## 常用事件

```javascript
// 鼠标事件
el.addEventListener("click", handler)
el.addEventListener("dblclick", handler)
el.addEventListener("mouseenter", handler)  // 不冒泡
el.addEventListener("mouseleave", handler)  // 不冒泡
el.addEventListener("mouseover", handler)   // 冒泡
el.addEventListener("mousemove", handler)

// 键盘事件
document.addEventListener("keydown", e => {
    console.log(e.key)       // "Enter", "a", "ArrowUp"
    console.log(e.keyCode)   // 数字键码（旧式）
    if (e.key === "Enter") { ... }
    if (e.ctrlKey && e.key === "s") { ... }  // Ctrl+S
})
document.addEventListener("keyup", handler)

// 表单事件
input.addEventListener("input", e => console.log(e.target.value))
input.addEventListener("change", handler)   // 失焦后触发
input.addEventListener("focus", handler)
input.addEventListener("blur", handler)
form.addEventListener("submit", e => {
    e.preventDefault()  // 阻止表单默认提交
    // 处理数据
})

// 窗口事件
window.addEventListener("load", handler)             // 页面完全加载
document.addEventListener("DOMContentLoaded", handler) // DOM加载完成
window.addEventListener("resize", handler)
window.addEventListener("scroll", handler)
```

## event 对象

```javascript
el.addEventListener("click", function(e) {
    e.target          // 触发事件的元素
    e.currentTarget   // 绑定事件的元素
    e.type            // 事件类型 "click"
    e.clientX         // 鼠标相对视口的X坐标
    e.clientY         // 鼠标相对视口的Y坐标
    e.pageX           // 鼠标相对页面的X坐标

    e.preventDefault()   // 阻止默认行为（如链接跳转）
    e.stopPropagation()  // 阻止事件冒泡
});
```

## 事件冒泡与委托

```javascript
// 事件冒泡：子元素事件会向上传播到父元素
document.querySelector("#parent").addEventListener("click", e => {
    console.log("parent clicked");
});
document.querySelector("#child").addEventListener("click", e => {
    console.log("child clicked");
    // e.stopPropagation()  // 加这行就不会冒泡到 parent
});
// 点击 child 时：先打印 "child clicked"，再打印 "parent clicked"

// 事件委托：利用冒泡，把事件绑定在父元素上
// 适合动态添加的子元素
document.querySelector("#list").addEventListener("click", e => {
    if (e.target.tagName === "LI") {
        console.log("点击了：", e.target.textContent);
    }
});
```

## 常见坑

- `e.target` 是实际点击的元素，`e.currentTarget` 是绑定事件的元素，委托时要注意区分
- `mouseenter` / `mouseleave` 不冒泡，`mouseover` / `mouseout` 会冒泡
- 表单 `submit` 事件记得 `e.preventDefault()`，否则页面会刷新
- 绑定多个相同事件用 `addEventListener`，用 `onclick` 会覆盖
