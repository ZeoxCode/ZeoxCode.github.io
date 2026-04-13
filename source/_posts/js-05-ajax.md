---
title: JS 05 - Ajax 与表单交互
date: 2017-08-05
tags: [JavaScript, 前端]
categories:
  - 全栈开发
  - 前端
---

## XMLHttpRequest（传统方式）

```javascript
const xhr = new XMLHttpRequest();

xhr.open("GET", "https://api.example.com/users", true);  // true=异步

xhr.onreadystatechange = function() {
    if (xhr.readyState === 4) {       // 4 = 请求完成
        if (xhr.status === 200) {     // 200 = 成功
            const data = JSON.parse(xhr.responseText);
            console.log(data);
        } else {
            console.error("请求失败：", xhr.status);
        }
    }
};

xhr.send();

// POST 请求
xhr.open("POST", "/api/login", true);
xhr.setRequestHeader("Content-Type", "application/json");
xhr.send(JSON.stringify({ username: "alice", password: "123" }));
```

## Fetch API（现代方式）

```javascript
// GET
fetch("https://api.example.com/users")
    .then(response => {
        if (!response.ok) throw new Error("请求失败");
        return response.json();
    })
    .then(data => console.log(data))
    .catch(err => console.error(err));

// POST
fetch("/api/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ username: "alice", password: "123" })
})
    .then(res => res.json())
    .then(data => console.log(data));
```

## JSON

```javascript
// JS 对象 → JSON 字符串
const obj = { name: "Alice", age: 20 };
const json = JSON.stringify(obj);
// '{"name":"Alice","age":20}'

// JSON 字符串 → JS 对象
const parsed = JSON.parse('{"name":"Alice","age":20}');
parsed.name  // "Alice"

// 格式化输出
JSON.stringify(obj, null, 2)
// {
//   "name": "Alice",
//   "age": 20
// }
```

## 表单数据处理

```javascript
// 读取表单值
const form = document.querySelector("#myForm");
form.addEventListener("submit", function(e) {
    e.preventDefault();

    const username = document.querySelector("#username").value;
    const password = document.querySelector("#password").value;
    const gender = document.querySelector("input[name='gender']:checked")?.value;
    const hobbies = [...document.querySelectorAll("input[name='hobby']:checked")]
                    .map(el => el.value);

    // 用 FormData
    const formData = new FormData(form);
    formData.get("username");
    formData.getAll("hobby");

    // 发送
    fetch("/api/register", {
        method: "POST",
        body: formData  // 直接传 FormData，不用设 Content-Type
    });
});

// 表单验证
function validate() {
    const email = document.querySelector("#email").value;
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
        alert("邮箱格式不正确");
        return false;
    }
    return true;
}
```

## localStorage

```javascript
// 本地存储（关闭浏览器后数据保留）
localStorage.setItem("token", "abc123");
localStorage.getItem("token");   // "abc123"
localStorage.removeItem("token");
localStorage.clear();

// 存对象（需序列化）
localStorage.setItem("user", JSON.stringify({ name: "Alice" }));
const user = JSON.parse(localStorage.getItem("user"));

// sessionStorage（关闭标签页后清除）
sessionStorage.setItem("temp", "data");
```

## 常见坑

- `fetch` 只在网络错误时 reject，HTTP 4xx/5xx 不会 reject，要手动检查 `response.ok`
- `XMLHttpRequest` 的 `readyState` 有 0~4 五个状态，只有 4 才是完成
- 跨域请求受同源策略限制，需要服务端设置 `CORS` 头
- `localStorage` 只能存字符串，存对象一定要 `JSON.stringify` / `JSON.parse`
