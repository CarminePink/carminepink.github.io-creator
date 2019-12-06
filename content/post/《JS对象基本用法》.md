---
title: "《JS对象基本用法》"
date: 2019-10-15T16:57:12+08:00
draft: false
---

# JS 对象

## 定义

- 无序的数据集合
- 键值对的集合

## 写法

1. `let obj = { 'name': 'frank', 'age':18}`

2. `let obj = new Object({'name': 'frank', 'age':18})`

需要注意的是要和下面的代码区分开来

```JavaScript
{
    foo:'a';
}
```

这不是一个对象，foo 只是一个 label 标签，它有一个语句是字符串'a'

## 需要注意的细节

1. 对象里的键名都是**字符串**(注意都是！！！)，不是标识符，所以可以包含任意字符
2. 键名的引号可以省略，省略后只能写标识符(但是数字是个特例，也可以直接写)
3. 就算引号省略了，键名仍然是个**字符串**！！！
4. 当键名中有特殊字符时，不要省略引号

举个例子

```JavaScript
var obj = {
    'name':'frank',
    age:18,      //age省略了引号，但是age还是字符串
    'a b':'hi',  //字符串"a b"作为键名
    '':'abc'     //空字符串作为键名
    2:'222'      //数字2也是字符串"2"
}
```

综上所述，键名只会是**字符串！！！**

## 属性

键名实际上是叫做属性名(property)，键值实际上叫做属性值(value)
举个例子

```JavaScript
let obj = {
    'name':'jack',
    'age': 18
}
```

上述代码 obj 对象的属性分别是 name、age,对应的属性值分别是'frank'、18

- 如果要用变量的值作为属性名的话就要用到中括号[]
  举个例子

```JavaScript
let p = 'name'
let obj = {
    p:'frank'
}
```

上面代码 obj 的属性名是字符串'p',因为写的时候 p 其实省略了引号，所以它实际上还是一个字符串。

如果要用变量 p 的值作为属性名的话需要这样写

```JavaScript
let p = 'name'
let obj = {
    [p]:'frank'
}
```

这样的话 obj 的属性名就是字符串'name',加了中括号就是取变量的值。

再举个例子

```JavaScript
let obj = {
    [1+2+3+4]:'值是十'
}
```

上述代码 obj 的属性名是什么?

1. 首先先求中括号里的值，1+2+3+4===10,然后会把值 10 变成字符串(因为所有属性名都是字符串！！！)
2. 所以 obj 的属性名是字符串"10"

## 对象的隐藏属性

- 每个对象都有一个隐藏属性
- 这个隐藏属性存储着其**共有属性组成的对象**的**地址**
- 共有属性组成的对象叫做原型
- 隐藏属性存储着原型的地址

举个例子
`var obj = {'name' : 'frank'}`
用图说明 obj 的隐藏属性和原型

![图示](/images/yincang.jpg)

## 属性的删除

```JavaScript
let obj = {
    name: 'frank',
    age: 18
}
```

1. `delete obj['name']`
2. `delete obj.name`

上面两种方法都可以删除 obj 的 name 属性

- 要注意区分`delete obj['name']`和`obj.name === undefined`的区别

前者是把整个属性名和属性值都删除掉了，而后者的属性名还在，值变成了 undefined 罢了，其实就是修改属性。

- 判断一个属性是否在某对象中
  `'xxx' in obj`

若返回值为 true，说明属性'xxx'在对象 obj 中，返回 false 则不在。需要注意的是属性'xxx'一定要加引号，不加引号就是变量了。

- 注意 `obj.xxx === undefined`是不能判断'xxx'是否在对象 obj 中的

举个例子说明就明白了

```JavaScript
let obj1 = {
    'name':'frank'
}
obj1.age ===undefined //true
```

对象 obj 没有属性 age，但是通过`obj1.age ===undefined //true`的返回值却是 true，足以说明问题

## 属性的查看(读属性)

- 查看自身所有属性

  `Object.keys(obj)`

- 查看自身所有属性值

  `Object.values(obj)`

- 同时查看属性和属性值

  `Object.entries(obj)`

- 查看自身属性和共有属性

  `console.dir(obj)`

  `obj.__proto__` //只能查看共有属性

* 判断某个属性是否是该对象的自身属性还是共有属性

`obj.hasOwnProperty('xxx')`

举个例子

```JavaScript
let obj = {
    name:'frank',
    age:18
}
obj.hasOwnProperty('name')      //true
obj.hasOwnProperty('toString')    //false
```

返回 true 说明该属性是自身属性，返回 false 说明该属性是共有属性

- 查看单个属性

1. 中括号语法 `obj['name']`
2. 点语法 `obj.name`

优先使用中括号语法，因为使用点语法可能会忘记 `obj.name` 中的 name 是个字符串

- 注意的细节

`obj.name` 等价于 `obj['name']`,不等价于`obj[name]`

`obj.name`和`obj['name']`中的 name 是个**字符串**，`obj[name]`中的 name 是个**变量**

举个例子

```JavaScript
let list = ['name','age','gender']
let person = {
    name:'frank',
    age:18,
    gender:'man'
}
for(let i=0;i<list.length;i++){
    let name = list[i]
    console.log(?)
}
请问console.log中应该填什么才能打印出person的所有属性

A person.name       B person[name]
```

上述代码的答案是 B,因为`console.log(person[name])`,这个 name 是一个变量，他会分别取到'name'、'age'、'gender'，所以会把 person 的所有属性都打印出来，A 选项中`console.log(person.name)`中的 name 就是 person 的属性'name'，所以不管打多少次，都是打印名字。

## 原型

- 每个对象都有原型

1. 原型里存着该对象的共有属性
2. 比如 obj 的原型就是一个对象，里面存着 toString、valueOf 等共有属性
3. `obj.__proto__`就存着这个原型的地址

- 每个原型也是一个对象

1. 所有对象的原型也有原型
2. obj={}的原型就是所有对象的原型
3. 该原型包含所有对象的共有属性，是所有**对象的根**
4. 该原型也有原型，是 null

- 每个对象都有原型，每个原型也是一个对象 => 每个原型也有原型

## 修改或删除属性(写属性)

- 直接赋值

举个例子

```JavaScript
var obj = {name: 'frank',age:18}
```

1. `obj.name = 'jack'`
2. `obj['name'] = 'jack'`
3. `obj['na'+'me'] = jack'`
4. ```JavaScript
   let key = 'name'
   obj[key] = 'jack'
   ```

- 批量赋值

`Object.assign(obj,{name:'jack',age:20,gender:'man'})`

第一个参数 obj 指明要修改的对象，第二个参数是自己增加/修改的属性，若 age 属性在 obj 中已经存在了，这个时候就是对 age 的修改，而不是增加了。

- 无法通过自身修改/增加共有属性

举个例子

```JavaScript
let obj1 = {}
let obj2 = {}
obj1.toString = 'xxx'
```

上述代码`obj2.toString`并不会受到影响，还是指向原型，`obj1.toString = 'xxx'`只会在 obj1 自身属性上增加一个 toString 的属性，值是'xxx'，画个内存图理解一下

![图示](/images/toString.jpg)

- 但是还是可以强行修改共有属性

```JavaScript
let obj = {}
obj.__proto__.toString = 'xxx'
那么
Object.prototype.toString = 'xxx'
```

不要修改原型，会引发很多问题

- 修改某对象的隐藏属性`__proto__`

1. 推荐使用`Object.create()`

举个例子

```JavaScript
let common = {国籍:'中国',肤色:'黄色'}
let obj = Object.create(common)
```

那么 obj 的隐藏属性就指向了 common 这个对象，`obj.__proto__`存储的就是 common 对象的地址。

## 总结

- 删除

1. `delete obj['xxx']`或者`delete obj.xxx`
2. `'xxx' in obj`
3. `obj.hasOwnProperty('xxx')`

- 查看

1. `Object.keys(obj)`
2. `Object.values(obj)`
3. `obj['xxx']`
4. `obj.xxx`

- 修改

1. 改自身

`obj['name'] = 'jack'`

2. 批量改

`Object.assign(obj,{age:18})`

3. 改共有属性

```JavaScript
obj.__proto__.toString = 'xxx' //不推荐
或者  Object.prototype.toString = 'xxx'
```

4. 改原型

```JavaScript
obj.__proto__ = common  //不推荐
或者 let obj = Object.create(common)
```

- 增加

基本同上，已有属性则改，没有的属性就是增。
