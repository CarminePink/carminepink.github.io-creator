---
title: "《JQuery设计模式》"
date: 2019-11-19T20:36:49+08:00
draft: false
---

jQuery 是目前使用最广泛的 JavaScript 库了，所以我认为学习 jQuery 函数库是十分重要的。

## 什么是 jQuery

jQuery 的设计思想就是：

1. window.jQuery 是提供给我们的一个**全局函数**
2. jQuery('选择器')用于获取对应的元素
3. 但是却并不返回这些元素
4. 而是**返回一个对象**，这个对象被称之为 jQuery 构造出来的对象(简称 jQuery 对象)
5. **这个对象可以操作对应的元素**

jQuery 并不能称为严格意义上的构造函数，因为它没有用 new。

我通过自己的学习和学习了阮一峰的[《jQuery 设计思想》](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)自己总结了 jQuery 的用法。

## 选择元素

使用 jQuery 首先把一个选择器放入到 jQuery()中即可得到可以操作对应元素的对象(api)，所以下面虽然我写的是得到 xxx 元素，但是我们要知道实际上是操作 xxx 元素的对象(api)

选择器可以是 CSS 选择器

- `$(document)` //选择整个文档元素
- `$('#xxx')` //选择 id 为 xxx 的元素
- `$('.xxx')` //选择 class 为 xxx 的元素
- `$('input[name=first]')` //选择 name 属性等于 first 的 input 元素

选择器也可以是 jQuery 特有的表达式

- `$(a:first)` //选择第一个 a 标签元素
- `$(div:visible)` //选择可见的 div 元素
- `$(tr:odd)` //选择表格中的奇数行
- `$(div:gt(2))` //选择所有的 div 元素，除了前三个

## 对选择的元素进行筛选

jQuery 提供**过滤**器，对**结果集**进行筛选

- `$('#xxx').find('.red')` //选择#xxx 里面 class 为 red 的元素
- `$('div').has('p')` //选择包含 p 标签的 div 元素
- `$('div').not('.xxx')` //选择所有 class 不等于 xxx 的 div 元素
- `$('div').filter('.xxx')` //选择 class 等于 xxx 的 div 元素
- `$('div').first()` //选择第一个 div 元素
- `$('div').eq(2)` //选择第三个 div 元素

## 对选择的元素再次选择其附近的相关元素

- `$('div').next('p')` //选择 div 后面的第一个 p 标签
- `$('div').parent()` //选择 div 的父元素
- `$('div').children()` //选择 div 的子元素
- `$('div').siblings()` //选择 div 的兄弟元素,不包括自己
- `$('div').closest('form')` //选择里 div 最近的那个父级 form 元素

## 链式操作

jQuery 选中一个元素后可以对其进行一连串的操作，举个例子

```JavaScript
$('div')  //找到div元素
    .find('.red') //找到其中class为red的元素
    .eq(2) //选择其中的第三个元素
    .children() //找到它的所有子元素
```

jQuery 之所以可以进行链式操作的原因就是每次返回的不是对应的元素，而是操作对应元素的对象(api)。

jQuery 还提供了 end()方法，可以使得**结果集**后退一步，还是拿上面的代码举例子

```JavaScript
$('div')  //找到div元素
    .find('.red') //找到其中class为red的元素
    .eq(2) //选择其中的第三个元素
    .html('Hello') //对其html内容进行赋值Hello
    .end()  //退回到选择div中class为red的元素这一步
    .eq(1)  //选择其中的第二个元素
    .children() //找到它的所有子元素
```

我自己的理解是，上面代码最近的结果集是.eq()，在其基础上后退一步就到了.find()，所意识退回到了.find 这一步，而不是退回到了.eq 这一步。

## 对选择的元素进行取值或赋值

jQuery 对选择的元素进行取值(getter)、赋值(setter)是通过同一个函数实现的,到底是取值还是赋值是通过参数个数决定的，这就是重载。

- `$('h1').html()` //html()没有参数，所以这是取值
- `$('h1').html('Hello')` //html()有参数 Hello，所以这是赋值

常见的取值和赋值函数

```JavaScript
.html() //取出或设置选择元素的html内容
.text()  //取出或设置选择元素的文本内容
.attr()  //取出或设置选择元素的属性值
.width()  //取出或设置选择元素的宽度
.height()  //取出或设置选择元素的高度
```

不过有些细节要注意，一般\$('xxx')选择元素选择的结果可能有多个，如果结果集包含多个元素的话，
进行赋值的时候，就是对结果集里的所有元素进行赋值，取值的话，只取出结果集中的第一个元素。(text()是个例外，不管取值还是赋值都是对结果集里所有元素进行改变)

## 对选择的元素进行移动

jQuery 有两种方法对选择的元素进行移动，一种是直接移动选择的元素，到我们想要的位置，第二种是移动其附近的元素，让选择的元素
达到我们想要的位置。

举个例子，假设我们现在想把一个 div 元素移动到一个 p 标签的后面。有两种方法。

1. 使用 insertAfter(),把 div 标签插入到 p 标签的后面

   `$('div').insertAfter('p')`

2. 使用 insertBefore(),把 p 标签插入到 div 标签的前面

   `$('p').insertBefore('div')` 或 `$('p').After('div')`

上面两种方法得到的结果是一样的，都是把 div 元素移动到了 p 元素的后面，但是有一个重要的区别是，第一种方法返回的是操作 div 元素的对象，第二种方法返回的是操作 p 元素的对象，两种方法返回的对象所操作的元素不同。

一共有四对操作元素移动的方法

- `$('div').insertAfter()` 和 `.After('div')` // 把 div 元素插入到目标元素的后面
- `$('div').insertBefore()` 和 `.Before('div')` //把 div 元素插入到目标元素的前面
- `.appendTo()` 和 `.append()` //对于.appendTo(),函数前面的是要插入的内容，函数里面的参数是要插入到谁里面，而.append()则是反过来的
- `.prependTo()` 和 `.prepend()` //理解和上面的 appendTo()差不多，细微的区别看下面的代码

举个例子

```HTML
<h2>Greetings</h2>
<div class="container">
  <div class="inner">Hello</div>
  <div class="inner">Goodbye</div>
</div>
```

```JavaScript
$('<p>Test</p>').appendTo('.inner');  //把p标签插入到class为inner的标签里面
//注意class为inner的标签里面原本就有Hello这个内容，所以用appendTo()插入的内容是插在Hello的后面，
//如果是用.prependTo()的话插入的内容就在Hello前面了
```

结果就是

```HTML
<h2>Greetings</h2>
<div class="container">
  <div class="inner">
    Hello
    <p>Test</p>
  </div>
  <div class="inner">
    Goodbye
    <p>Test</p>
  </div>
</div>
```

## 对选择的元素进行复制、删除和创建

1. 创建新的元素很简单，直接用`$(<div>你好</div>)`，就相当于创建了一个 div 元素
2. 复制元素用`.clone()`
3. 删除元素有两种方法

   - `.remove()` 和 `.detach()`

   两者的区别在于 detach 保留了被删元素的事件，如果还需要用到被删元素的话，可以再次插入，而 remove 则没有保留，删了就是删了，就消失了。

4. `.empty()` 用于移除所选元素的所有子元素，包括元素里的文本，因为元素里的文本字符串也会被认为是子元素，就好比空格。

举个例子

```HTML
<div class="container">
  <div class="hello">Hello</div>
  <div class="goodbye">Goodbye</div>
</div>
```

```JavaScript
$('.hello').empty();
```

结果是 Hello 文本也被移除了

```HTML
<div class="container">
  <div class="hello"></div>
  <div class="goodbye">Goodbye</div>
</div>
```
