---
title: JS 02 - 函数与作用域
date: 2017-07-12
tags: [JavaScript, 前端]
categories:
  - 全栈开发
  - 前端
---

## 函数定义

```javascript
// 函数声明（有提升）
function add(a, b) {
    return a + b;
}

// 函数表达式（无提升）
const multiply = function(a, b) {
    return a * b;
};

// 箭头函数（ES6，无自己的 this）
const square = x => x * x;
const greet = (name) => `Hello, ${name}`;
const sum = (a, b) => {
    let result = a + b;
    return result;
};
```

## 默认参数与剩余参数

```javascript
// 默认参数
function greet(name = "World") {
    return `Hello, ${name}!`;
}
greet()          // "Hello, World!"
greet("Alice")   // "Hello, Alice!"

// 剩余参数
function sum(...nums) {
    return nums.reduce((acc, n) => acc + n, 0);
}
sum(1, 2, 3, 4)  // 10
```

## 作用域

```javascript
// 全局作用域
var global = "global";

function outer() {
    var outerVar = "outer";

    function inner() {
        var innerVar = "inner";
        console.log(global);    // "global"（可访问）
        console.log(outerVar);  // "outer"（可访问）
        console.log(innerVar);  // "inner"
    }

    // console.log(innerVar);  // 报错，inner 的变量外部不可见
    inner();
}

// let/const 的块级作用域
{
    let blockVar = "block";
    const CONST = 100;
}
// console.log(blockVar);  // 报错，块外不可见
```

## 闭包

```javascript
function counter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}

const c = counter();
c()  // 1
c()  // 2
c()  // 3
// count 变量被内部函数引用，不会被回收

// 实际应用：创建私有变量
function createAccount(initial) {
    let balance = initial;
    return {
        deposit(amount)  { balance += amount; },
        withdraw(amount) { balance -= amount; },
        getBalance()     { return balance; }
    };
}
const acc = createAccount(100);
acc.deposit(50);
acc.getBalance();  // 150
```

## this

```javascript
// 普通函数：this 由调用方式决定
const obj = {
    name: "Alice",
    greet: function() {
        console.log(this.name);  // "Alice"
    },
    greetArrow: () => {
        console.log(this.name);  // undefined（箭头函数没有自己的 this）
    }
};

// 构造函数中的 this
function Person(name) {
    this.name = name;
}
const p = new Person("Bob");
p.name  // "Bob"

// 手动绑定 this
function sayName() {
    console.log(this.name);
}
sayName.call({name: "Alice"});   // "Alice"
sayName.apply({name: "Bob"});    // "Bob"
const bound = sayName.bind({name: "Charlie"});
bound();  // "Charlie"
```

## 常见坑

- 箭头函数没有自己的 `this`，不能用作构造函数，不能用 `new`
- `var` 在循环中共享同一个变量，用 `let` 避免经典的闭包陷阱
- 函数声明会提升，函数表达式不会，调用时要注意顺序
- 严格模式下（`"use strict"`）普通函数内 `this` 是 `undefined`，不是 `window`
