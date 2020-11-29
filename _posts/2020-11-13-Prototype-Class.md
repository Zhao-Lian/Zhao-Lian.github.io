---
layout: post
title: '原型、原型链与Class'
date: 2020-11-13
author: 廉兆
categories: 技术
cover: '/assets/img/cover/sky-690293_640.jpg'
tags: JavaScript ES6 继承
---

> JS 中的原型及继承的实现

### 一句话介绍

#### 原型对象

原型对象是一个对象，其中包含了类型对象共有的属性和方法，构造函数可以基于一个相同的原型，生成不同的对象。这些对象都可以访问共同原型对象上的属性和方法。

#### 原型链

原型对象抽离出实例不变的属性和方法，构造函数根据原型对象生成新的对象，新的对象可以访问原型对象上的属性和方法，原型对象又有自己的原型对象，因此，新对象也可以对此进行访问，这种链式的访问就是原型链。原型链最终指向 null。

#### Class

class 是 ES6 中基于原型的方式实现的类的语法糖，底层原理依旧是原型和原型链，但是写法更简洁。

### 基础概念

我们首先要搞清楚一个问题：对象是怎么来的？

对象实际上是通过一种机器生产出来的，这种机器就叫做构造函数，构造函数想要生成一个对象，需要知道生成的对象长成什么样子，所以，需要有参照，这个参照就是原型对象。

```js
// 构造函数
let Animal = function (name) {
  this.name = name
}
// 原型
Animal.prototype.sayName = function () {
  console.log(this.name)
}
// 实例
let cat = new Animal('cat')
```

其中涉及到三个不同的概念：

#### 构造函数

构造函数是生成对象的机器，参照的模板是原型对象，构造函数有一个属性`prototype`指向所参照的原型对象

#### 原型对象

原型对象是生成对象的模板，有一个属性`constructor`指向利用此模板的构造函数

#### 实例对象

实例对象是构造函数根据模板生成的对象，有一个属性`__proto__`指向它的模板对象

```js
Animal.prototype.constructor === Animal // true
cat.__proto__ === Animal.prototype // true
cat.__proto__.constructor === Animal // true
```

### 继承

#### 组合继承

```js
// 组合继承(综合了原型链和盗用构造函数)
// 基本思路是使用原型链继承原型上的属性和方法，通过盗用构造函数继承实例属性
// 这种继承方式优点在于构造函数可以传参，不会与父类引用属性共享，可以复用父类的函数，但是也存在一个缺点就是在继承父类函数的时候调用了父类构造函数，导致子类的原型上多了不需要的父类属性，存在内存上的浪费。
function SuperType(name) {
  this.name = name
  this.colors = ['red', 'blue', 'green']
}
SuperType.prototype.sayName = function () {
  console.log(this.name)
}
function SubType(name, age) {
  // 使用盗用构造函数的方式实现对于属性的继承，防止子实例之间共享引用类型的属性
  SuperType.call(this, name)
  this.age = age
}
// 使用原型对象的方式实现方法的继承
SubType.prototype = new SuperType()
SubType.prototype.constructor = SubType

SubType.prototype.sayAge = function () {
  console.log(this.age)
}

let instance1 = new SubType('one', 1)
instance1.sayName()
instance1.colors.push('black')
instance1.colors // ["red", "blue", "green", "black"]

let instance2 = new SubType('two', 2)
instance2.sayName()
instance2.colors // ["red", "blue", "green"]
```

#### 寄生组合继承

```js
// 寄生式组合继承
// 解决了组合继承的问题，不会多次调用构造函数
function SuperType(name) {
  this.name = name
  this.colors = ['red', 'blue', 'green']
}
SuperType.prototype.sayName = function () {
  console.log(this.name)
}
function SubType(name, age) {
  // 使用盗用构造函数的方式实现对于属性的继承，防止子实例之间共享引用类型的属性
  SuperType.call(this, name)
  this.age = age
}
// 使用寄生的方式实现方法的继承
// Object.create方法接受两个参数，作为新对象原型的对象，以及给新对象定义额外属性的对象（可选）
SubType.prototype = Object.create(SuperType.prototype)
SubType.prototype.constructor = SubType

SubType.prototype.sayAge = function () {
  console.log(this.age)
}

let instance1 = new SubType('one', 1)
instance1.sayName()
instance1.colors.push('black')
instance1.colors // ["red", "blue", "green", "black"]

let instance2 = new SubType('two', 2)
instance2.sayName()
instance2.colors // ["red", "blue", "green"]
```

### Class

```js
class Person {
  // constructor构造函数，每次使用new操作符创建一个类的新实例时，都会调用这个函数
  constructor(name) {
    this.name = name || 'unknow'
    // 添加到this的所有内容都会存在于不同的实例上
    this.locate = function () {
      console.log('instance', this) // this指向实例
    }
  }
  // 定义在类的原型对象上
  sayName() {
    console.log(this.name, this) // this指向实例
  }
  // 定义在类本身上，通过实例无法访问
  static locate() {
    console.log('class static', this) // this指向类
  }
}
// extends关键字表示继承
// 继承可以继承静态的属性和方法
class Man extends Person {
  constructor(name) {
    // super调用父类的构造函数，返回实例赋值给this，相当于Person.call(this, value)
    super(name)
    this.gender = 1
  }
}
let lee = new Person()
lee.locate() // instance Person {name: "unknow", locate: ƒ}
Person.prototype.sayName() // undefined {constructor: ƒ, sayName: ƒ}
lee.sayName() // unknow Person {name: "unknow", locate: ƒ}
Person.locate() // class static class Person {...}

let joe = new Man('Joe')
joe.locate() // instance Man {name: "Joe", gender: 1, locate: ƒ}
joe.sayName() // Joe Man {name: "Joe", gender: 1, locate: ƒ}
Man.locate() // class static class Man extends Person {...}
```

### 参考资料

[JavaScript 世界万物诞生记](https://zhuanlan.zhihu.com/p/22989691)
