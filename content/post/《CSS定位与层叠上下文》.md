---
title: "《CSS定位与层叠上下文》"
date: 2019-09-28T15:26:10+08:00
draft: false
---

# CSS 定位

- 首先我们得知道 CSS 中的 background: #xxx;背景色的范围是 border 的外边沿围成的一块区域。

- 一个 div 的分层如图
  ![三维视图](/images/3d.jpg)

1. 如果同时在块级子元素和内联子元素中出现了文字，那么是按照代码的先后顺序来覆盖文字的。
2. 只有浮动元素会被覆盖。

# Position

position 有五个属性值。

1. static(默认值)

```CSS
.container{
    position:static;
}
```

2. relative(相对定位)

```CSS
.container{
    position:relative;
}
```

position:relative;一般有两种用法

- 用作位移(很少用)，比如

```CSS
.container{
    position:relative;
    top:5px;
    left:5px
}
```

- 用作 position:absolute;的父亲，之后会明白。

3. absolute

```CSS
.container{
    position:absolute;
}
```

!!!使用 position:absolute;之后，一定要在其祖先中的某个标签加上 position:relative;

- 一般用于脱离原来的位置，比如另起一层(对话框的关闭按钮)，又或者是鼠标提示。
  [样例代码](http://js.jirengu.com/yejiw/1/edit?html,css,output)

* 首先要声明的一点是，absolute 不是相对于 relative 定位的，而是相对于它祖先元素中最近的一个定位元素定位的。
  那么何为定位元素呢？定位元素就是 position 不是 static 的元素就是定位元素。
  例如

```HTML
<div class="grandparent">
    <div class="parent">
      <div class="son"></div>
    </div>
</div>
```

```CSS
.son{
  position:absolute;
}

.parent{
  position: relative;
}
```

上面的代码.son 是相对于他祖先中的.parent 定位的，因为.son 的祖先元素中，最近的定位元素是.parent

- 如果一个元素的 position:absolute;那么最好(一定)要给该元素加上 top:~/left:~，因为如果不规定它的位置的话，在部分浏览器里面会位置错乱。
- 要活用 left:100%/50%; bottom:50%/100%;
- position:absolute/relative 都可以配合 z-index 使用来调整层级

* 默认的 z-index:auto;也即不创建新的层叠上下文
* z-index 的取值可以有-1/0/1/2;

4. fixed(相对于视口)

```CSS
.container{
    position:fixed;
}
```

- 常用与烦人的广告、回到顶部的按钮，因为它的位置是相对于视口的，不管界面怎下滑，他始终处于界面的固定位置。
- 但是存在一些属性如 transform 会影响其相对于视口的功能。

5. sticky(粘滞定位)

```CSS
.container{
    position:sticky;
}
* 这个属性用的很少，因为很多浏览器目前不兼容。
```

# 层叠上下文

- 我们要继续看这张图帮助我们理解![三维视图](/images/3d.jpg)

从上图可以看出，定位元素的层级是最高的，也即处于最上面。比内联子元素还要高。但是有两点要注意

1. 定位元素之所以层级更高，因为他们可以改变 z-index 的值，在层叠上下文中，默认的值是 0，值越大，层级越高。
2. 但是如果某定位元素的 z-index 的值为负值，那么它的层级将会比 background 还要低，就是最低的。

## 如何理解层叠上下文

- 每个层叠上下文可以看成是一个小世界(作用域)
- 每个小世界互不干扰，也即在该世界内的元素的 z-index 不能和其他世界元素的 z-index 进行比较(也不是说不能比较，准确的说是没有可比性，也即 z-index 值大的层级不一定高)
- 处在同一个世界的元素的 z-index 才有可比性

比如

```HTML
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
<body>
  <div class="container">
    <div class="a">
      层叠上下文
      <div class="a1">a1</div>
    </div>
    <div class="b">
      <div class="b1">b1</div>
    </div>
  </div>
</body>
</html>
```

```CSS
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}
.container{
  border: 1px solid black;
  height:150px;
}
.a{
  border: 1px solid green;
}
.a1{
  height:50px;
  width:50px;
  background: blue;
  position:relative;
  z-index:15;
}

.b1{
  height:50px;
  width:50px;
  background: red;
  position:relative;
  z-index:10;
  top:-20px;
}
```

![效果图如下](/images/cengdie1.png)
此时.a1 的 z-index 为 15 ，.b1 的 z-index 为 10，这时他们都处于 html(根元素)创建的层叠上下文中，所以.a1 的内容会覆盖.b1 的内容。

但是如果在.a1 的父元素.a 里面加上

```CSS
.a{
  border: 1px solid green;
  position: relative;
  z-index:1;
}
```

这时候.a1 就存在于.a 创建的层叠上下文中了，因为.a 创建的层叠上下文 z-index 为 1，所以即使.a1 的 z-index 为 15，也是在.a 的 z-index 为 1 为基础的世界里，所以此时.b1 的层级又变得比.a1 高了，所以覆盖了.a1 的内容
![效果图如下](/images/cengdie2.png)

## 有哪些属性可以创建层叠上下文呢

1. 根元素 HTML
2. z-index 的值不为 auto 的 绝对/相对定位元素
3. opacity 不为 1 的元素
4. transform 属性值不为 none 的元素
5. filter 值不为 none 的元素
6. perspective 的值不为 none 的元素
7. position:fixed/sticky(不论 z-index 的值为多少，即使是 auto 也会创建)
8. 一个 z-index 的值不为 auto 的 flex 项目(flex item),也即父元素的 display:flex/inline-flex;

- 如果某个元素创建了层叠上下文。那么即使是负的 z-index 也逃不出该元素，因为被困在这个世界里了。
