---
layout: post
title: 'JS原始数据类型之Symbol'
date: 2020-11-06
author: 廉兆
categories: 技术
cover: '/assets/img/cover/clouds-over-the-mountains-5031782_640.jpg'
tags: JavaScript ES6
---

## Symbol

### 一句话介绍

Symbol，意为**标识符**，是 ES6 引入的一种新的原始数据类型，每个 Symbol 类型的值都是唯一的，用来作为对象属性的标识符，可以保证对象的属性名之间不会冲突。

### 基础用法

#### 定义一个 Symbol 类型的值

`Symbol([description])`会返回一个唯一的标志符，每次调用的结果的都不相同

可以选择添加一个字符串类型的参数，用来进行描述。

```javascript
let sym1 = Symbol()
let sym2 = Symbol('foo')
let sym3 = Symbol('foo')
```

即使使用相同的描述，生成的两个 Symbol 值也不相等、

```javascript
Symbol('foo') === Symbol('foo') // false
```

### 使用场景

```javascript
let key = Symbol()

let obj = {}
obj[key] = 'value'

console.log(obj[key]) // 'value'
```

Symbol 作为对象属性名时不能用`.`运算符，要用方括号。因为`.`运算符后面是字符串，所以取到的是字符串`key`属性，而不是 Symbol 值`key`属性。

```js
console.log(obj[key]) // 'value'
console.log(obj.key) // undefined
```

### 参考资料

[MDN 关于 Symbol 的介绍](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)

[菜鸟教程](https://www.runoob.com/w3cnote/es6-symbol.html)
