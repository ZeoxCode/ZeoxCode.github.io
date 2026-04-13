---
title: JS 01 - 基础语法与数据类型
date: 2017-07-05
tags: [JavaScript, 前端]
categories:
  - 全栈开发
  - 前端
---

## 变量声明

```javascript
var x = 10;      // 函数作用域，可重复声明，有变量提升
let y = 20;      // 块级作用域，不可重复声明（推荐）
const PI = 3.14; // 块级作用域，声明后不能重新赋值（推荐）
```

## 数据类型

```javascript
// 基本类型
typeof 42          // "number"
typeof "hello"     // "string"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object"（历史遗留bug）
typeof Symbol()    // "symbol"

// 引用类型
typeof {}          // "object"
typeof []          // "object"
typeof function(){} // "function"
```

## 字符串

```javascript
let s = "Hello";
let s2 = 'World';
let s3 = `Hello, ${s2}!`;  // 模板字符串（ES6）

s.length           // 5
s.toUpperCase()    // "HELLO"
s.toLowerCase()    // "hello"
s.indexOf("l")     // 2
s.slice(1, 3)      // "el"
s.replace("l", "r") // "Herlo"（只替换第一个）
s.split("")        // ["H","e","l","l","o"]
s.trim()           // 去首尾空白
s.includes("ell")  // true（ES6）
s.startsWith("He") // true（ES6）
```

## 数组

```javascript
let arr = [1, 2, 3, 4, 5];

arr.length         // 5
arr.push(6)        // 末尾添加，返回新长度
arr.pop()          // 删除末尾，返回删除元素
arr.unshift(0)     // 开头添加
arr.shift()        // 删除开头
arr.splice(1, 2)   // 从索引1删2个
arr.splice(1, 0, 10, 11)  // 在索引1插入10,11

arr.indexOf(3)     // 2
arr.includes(3)    // true
arr.slice(1, 3)    // [2, 3]（不修改原数组）
arr.reverse()      // 反转（修改原数组）
arr.sort()         // 排序（默认字典序！）
arr.sort((a, b) => a - b)  // 数字升序

// 遍历
arr.forEach(item => console.log(item))
let doubled = arr.map(x => x * 2)
let evens = arr.filter(x => x % 2 === 0)
let sum = arr.reduce((acc, x) => acc + x, 0)
```

## 对象

```javascript
let person = {
    name: "Alice",
    age: 20,
    greet: function() {
        console.log("Hello, I'm " + this.name);
    }
};

person.name          // "Alice"
person["age"]        // 20
person.greet()       // Hello, I'm Alice

// 添加/修改属性
person.email = "alice@example.com";
person.age = 21;

// 删除属性
delete person.email;

// 判断属性是否存在
"name" in person     // true
person.hasOwnProperty("name")  // true
```

## 常见坑

- `==` 会做类型转换（`"1" == 1` 为 true），`===` 不转换，**总是用 `===`**
- `null == undefined` 为 true，`null === undefined` 为 false
- 数组的 `sort()` 默认按字符串排序：`[10, 9, 2].sort()` = `[10, 2, 9]`
- `var` 有变量提升，可以在声明前使用（值为 undefined），`let`/`const` 不行
