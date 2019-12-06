---
title: "《JS对象的分类》"
date: 2019-10-20T21:40:14+08:00
draft: false
---

## 构造函数(首字母大写)

- 构造函数，顾名思义，能够构造出一个对象的函数叫做构造函数。
- 若某个对象的属性 `constructor: xxx`，说明该对象是的构造函数就是 xxx。可以用`console.log(obj)`,查看对象 obj 的构造函数是谁。

* 每个函数都有一个叫 prototype 的属性，且 prototype 里有一个 constructor 属性，constructor 的值就是函数自身。

举个例子

```JavaScript
function fn(){}
console.dir(fn)

fn.prototype.constructor === fn //true
```

函数 fn 的结构图

![如图](/images/constructor.png)

- 下面用两个版本的代码来演示构造函数的优点

我们要创建一个对象正方形，它有自身属性 width，有共有属性 getArea()，和 getLength().

1. 用非构造函数实现

```JavaScript
function createSquare(width){
    let obj = Object.create(createSquare.squarePrototype)
    obj.width  = width
    return obj
}
createSquare.squarePrototype(){
    getArea(){return this.width * this.width}
    getLength(){return this.width * 4}
    constructor:createSquare
}
let square = createSquare(5)
```

2. 用构造函数实现

```JavaScript
function Square(width){
    this.width = width
}
Square.prototype.getArea= function(){
    return this.width * this.width
}
Square.prototype.getLength = function(){
    return this.width * 4
}
let square = new Square(5)
```

上述两种代码都满足条件，构造出了一个正方形对象。

我们可以总结一下 new Square()帮我们做了哪些事情

1. **自动创建了一个空对象**
2. **并把空对象的原型指向了构造函数.prototype**
3. **并且将该空对象作为 this 关键字运行构造函数**
4. **自动 return 该空对象**

那么构造函数做了哪些事呢

1. 构造函数本身是给我们要创建的对象添加其自身属性的
2. 构造函数.prototype 是负责保存创建的对象其共有属性的

我们自己写一个构造函数的例子

```JavaScript
function Dog(name){
    this.name = name
}
Dog.prototype.run = function(){
    console.log('狗在跑')
}
Dog.prototype.eat = function(){
    console.log('狗在吃')
}
let dog1 = new Dog('小花')
let dog2 = new Dog('拉斯')
```

我们上述代码创建了两个小狗对象分别是 dog1 和 dog2，他们都有自身属性 name，有共同属性 run 和 eat。

## 一个重要的结论(公式)

- 你是哪个构造函数构造出来的，你的原型就是构造函数的 prototype 属性指向的对象。(构造函数.prototype 存的是原型的地址)

举个例子

```JavaScript
let obj = new Object()

那么
obj.__proto__ === Object.prototype //true
```

所以公式就是

**`对象.__proto__` === `其构造函数.prototype`**

- 需要注意的一点是`构造函数.prototype 的原型`也即`构造函数.prototype.__proto__ === null`

## 对象也需要分类

- 有很多对象有一样的属性和行为，就需要把他们分为同一类，如 dog1 和 dog2
- 也有很多对象有不同的属性和行为，如 square、circle，就要把他们分为不同类

### 需要把类型和类区分开来

- 类型是 JS 数据的类型，总共有七种、四基两空一对象。
- 类是针对于对象的分类，有无数种，常见的有 Array、Function、Date 等

### class 语法

我们看下面两种代码都创建了一个 dog 对象

1. prototype 语法

```JavaScript
function Dog(name){
    this.name = name
}
Dog.prototype.run = function(){
    console.log('狗在跑')
}
let dog = new Dog('小花')
```

2. class 语法

```JavaScript
class Dog(name){
    constructor{
        this.name = name
    }
    run(){
        console.log('狗在跑')
    }
}
let dog = new Dog('小花')
```

class 语法引入了很多新的概念，建议先把 prototype 语法学会，当然 class 语法最好也要掌握。

## 这是我自己考虑了很久的一个问题

```JavaScript
let arr = [1,2,3]
我们会发现
arr.__proto__ === Array.prototype  //这是毫无疑问的，根据上面那个重要的公式也可推出来
然后
arr.__proto__.__proto__ ===Object.prototype
也即
Array.prototype.__proto__ ===Object.prototype

Object.prototype.__proto__ ===null

```

- Object 的原型是指`Object.__proto__` 而不是`Object.prototype`。`Object.prototype`只是保存着 Object 构造出来的所有对象的原型的地址。

* `Object.prototype`就是**所有对象的根**，但是`Object.prototype`的原型就是 null 了，也即`Object.prototype.__proto__ ===null`

* `Object.prototype`有可能不是第一层原型，比如上面代码创建的 arr 数组，arr 数组的第一层原型是`Array.prototype`,第二层原型才是`Object.prototype`

关于第三点，我想说一下我自己的想法，为什么 arr 的第二层原型是`Object.prototype` 呢？ 我认为首先 arr 是个数组，那么它的构造函数肯定是 Array，所以第一层原型应该没有异议，`arr.__proto__ === Array.prototype` 关键就是第二层原型，在之前的学习中我们知道**数组(Array)、函数(Function)、日期(Date)都不是单独的数据类型，他们都被归为 Object 对象这个数据类型一类中，所以虽然 arr 是个数组，但它同时也是个对象(数组对象)，又因为所有对象的根都是`Object.prototype`，**所以 arr 的第二层原型就是`Object.prototype`

- 那么 Object 的原型也即(`Object.__proto__`)是谁呢？ 我们得知道，Object，Array，Function 都是构造函数(函数对象)，所以既然是函数，那么肯定是由 Function 构造的，所以套用公式

`Object.__proto__ === Function.prototype`

同理

`Array.__proto__ === Function.prototype`

那么 Function 的原型又是谁呢？根据上面的推论，没错 Function 的原型就是它自身

`Function.__proto__ === Function.prototype`

**并不是 Function 自己构造了自己，而是浏览器构造了 Function，然后指定了它的构造者就是它自己**
