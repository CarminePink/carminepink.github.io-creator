---
title: "《JS语法》"
date: 2019-10-13T15:35:17+08:00
draft: false
---

## 表达式与语句

### 表达式
先举几个例子
1. 1+2的表达式的值为3
2. add(1,2)表达式的值为函数的返回值
3. console表达式的值为函数本身，因为你没有调用console函数
![如图所示](/images/biaodashi1.png)
4. console.log(3)表达式的值为undefined，因为console.log函数的返回值是undefined，打印出来的3只是一种记录而已，并不是它的值。
![如图所示](/images/biaodashi2.png)

* 表达式一般都有值，但是也不绝对
* 一个函数表达式的值就是该函数的返回值，

### 语句
也举一个例子
1. var a = 1
这是一个声明语句

* 语句可以有值，也可以没值
* 语句一般会改变环境(声明或赋值)

* 一般空格和回车都没有实际意义，所以怎么加空格和回车都可以，但是唯一特殊的一点就是不能在函数体的return后面加回车！比如
```JavaScript
function fn(){
    return
    3
}
fn()
```
那么最后的返回值会是undefined，因为上述代码会被JS自动变成
```JavaScript
function fn(){
    return undefined
    3
}
fn()
```
在return后面加回车，JS就会自动在return后面加上一个undefined

## 标识符
标识符的规则
1. 第一个字符可以是Unicode字母/$/_/中文(Unicode字母、美元符号、下划线、中文)
2. 后面的字符除了可以是上述的之外，还可以是数字
3. 总之就是第一个字符不能是数字！
举几个例子
```JavaScript
var _ = 1
var $ = 2
var 你好 = 3
```
上述变量名都是标识符

## if语句
* 语法
```JavaScript
if(表达式){
    语句1
}else{
    语句2
}
```
* 需要注意的特殊变态情况
1. 表达式里要看清楚是===还是=，如果是一个=，就是赋值
2. 语句1和语句2里面可以嵌套if else
例如
```JavaScript
var a = 2
if(a>1){
    if(a<100){
        console.log('a大于1小于100')
    }
}
```
又或是
```JavaScript
var a = 2
if(a<1){
}else if(a<10){
    console.log('a大于1小于10')
}
```

3. 注意缩进和省略
``` JavaScript
var a =1
if(a===2)
    console.log(a)
    console.log('a等于1')
```
最后打印出来的结果是'a等于1'，因为上面的代码首先省略了花括号，其次缩进也相当于多大了几个空格，之前说了空格和回车对代码没有影响，上面的代码等于下面的
``` JavaScript
var a =1
if(a===2){
    console.log(a)
}
console.log('a等于1')
```
所以理所当然的只打印'a等于1'


* 最推荐的没有歧义的写法是
```JavaScript
if(表达式){
    语句
}else if(表达式){
    语句
}else{
    语句
}
```

## switch语句
* 语法
```JavaScript
switch(fruit){
    case "banana":
        //语句
        break
    case "apple":
        //语句
        break
    default:
        //语句
}
```
最好不要省略break



## 问号冒号表达式
* 语法
```JavaScript
表达式1? 表达式2:表达式3
```
一般都是if else都只有一个语句的时候才会使用
比如
```JavaScript
function max(a,b){
    return a>b? a:b;
}
```

## &&短路逻辑
举例
```JavaScript
if(window.fn){
    console.log('函数fn存在')
}
```
上面的代码等价于
```JavaScript
window.fn && console.log('函数fn存在')
```
再举一个例子
```JavaScript
console&&console.log&&console.log('hi')
```
最后会打印出hi
* 综上所示，A&&B，若A为真，则执行B,且返回值或结果就是B
若A为假，则不执行后面的了，最好返回值或结果就是A
* &&短路逻辑相当于并且，只会取第一个假值或取最后一个真值。

## ||短路逻辑
举例 A||B相当于
```JavaScript
if(!A){
    B
}
```
就是当A不满足的时候才会执行B
再举一个例子,假设我们现在不知道a是否存在,a=a||100就相当于
```JavaScript
if(a){
    a=a
}else{
    a=100
}
```
100就是a的保底值

* 综上所述，||短路逻辑就是取第一个真值或者最后一个假值

## while语句
* 语法
```JavaScript
while(表达式){
    语句
}
```
如表达式为真，则执行语句，执行完后好要再次判断表达式的真假。

* 特殊变态情况
```JavaScript
var a = 0.1
while(a != 1){
    console.log(a)
    a = a + 0.1
}
```
上面的while循环是个死循环，是因为浮点数不精确导致的，a=0.1,a+0.1不一定等于0.2，可能是0.199999
![如图](/images/while.png)

## for循环
* 语法
```JavaScript
for(语句1;表达式1;语句2){
    循环体
}
```
* 特殊变态情况
```JavaScript
for(var i=0;i<5;i++){
    setTimeout(()=>{
        console.log(i)
    },0)
}
```
上述代码最后打印出来的结果是五个5
![如图](/images/for.png)
为什么是打印出五个5呢，因为setTimeout是过一会执行的意思，过一会之后i已经是5了，就好比
```JavaScript
var a =1
function fn(){
    console.log(a)
}
```
在执行fn()之前，我们是不能确定打印出来的a就是1，因为可能在fn()执行之前，a的值可能发生变化
就好像
```JavaScript
var a =1
function fn(){
    console.log(a)
}
setTimeout(fn)
a = 5
```
最后打印出来的a的值是5，而不是1，因为过一会执行fn，在这个过程中a的值已经改变了，变成了5
要是还不明白，就用更直白的例子来说明
* 假设我现在正在打游戏，老妈做好饭了来叫我过一会去吃饭，我肯定是先把手头上的这一把Dota打完才去吃饭，不可能挂机坑队友直接去吃饭吧，同理，setTimeout就是过一会，所以等for循环执行完了才会去打印，这时候i就是5
* 那为什么会打印5遍呢，假设明天早上ti比赛7点钟开始，你就定了一个明天7点的闹钟，但是害怕一个闹钟睡过了，于是你就定了5个7点的闹钟，那么到了第二天早上7点钟，你的五个闹钟都会响，所以同理，会打印5遍

* 如果for循环里的初始化语句使用let的话，那么会单独有一种逻辑，这里我还不清楚。

## break和continue
* break只会跳出当前最近的一个循环体。
```JavaScript
for(var i = 0;i<100;i++){
    for(var j = 10;j<20;j++){
        if(i===5){
            break;
        }
    }
    console.log(i)
}
```
上面的代码，当i===5时,只会跳出j这个for循环，i这个for循环还是要继续执行的。

* continue就是下一个/继续
```JavaScript
for(var i=0;i<10;i++){
    if(i%2===1){
        continue;
    }
    console.log(i)
}
```
![结果如图](/images/continue.png)
他就会打印出10以内的所有偶数，因为若i为奇数，他就会跳到下一个，但是不会跳出循环，所以就跳到了偶数。

## label语句
* 语法
```JavaScript
foo:{
    console.log(1)
    break foo
    console.log(2)
}
```
上面的foo就是一个标签，它有一个代码块，里面的代码就是打印出1

* 特殊变态情况
```JavaScript
{
    foo:1
}
```
上面的代码的意思就是，foo是一个标签，它有一个语句就是1

