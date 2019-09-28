---
title: "《CSS文档流与盒模型》"
date: 2019-09-27T15:36:17+08:00
draft: false
---

# CSS 文档流(Normal Flow)

在学习 CSS 之前首先要了解两个非常重要的概念，一个是文档流，另一个就是盒模型。

## 文档流

1. 文档流，顾名思义，就是文档中元素的流动方向，或者说是排列方向。

## 块级元素和内联元素

1. 内联元素(inline),如 span 标签，是从左到右排列的，排满一行才会换行继续排列，并且一个标签的内容可能会跨行显示。
2. 块级元素(block)，如 div 标签，是从上到下排列的，并且每一个 div 标签单独占据一行，即使该 div 标签的内容很少。
3. inline-block 元素，结合了内联元素和块级元素的特点，从左到右排列，但是一个标签的内容不会跨行显示。
   ![示例如图](/iamges/span.png)

- 需要注意的是，其实没有固定的内联/块级元素，因为可以人为的对其属性进行修改，比如

```CSS
span{display:block;}
```

给 span 标签修改如上属性，那么 span 标签就变成了块级元素。

- 不要在 inline 元素里写 block 元素

## 宽度

1. inline 元素的宽度是根据其标签内部 inline 元素内容之和决定的，不能用 width 指定宽度。比如

```HTML
<span>第一个span
    <span>第二个span</span>
</span>
```

它的宽度就是两个 span 标签的内容之和。

2. block 元素的宽度是默认的，默认 width：auto,默认就是一行能有多宽就占多宽，可以用 width 指定宽度。

3. inline-block 元素的宽度默认是其内部内容之和的宽度，但是也可以使用 width 指定宽度。

## 高度

1. inline 元素的高度是由 line-height 间接决定的，之所以是间接决定的原因是因为字体的样式也会影响 inline 元素的高度，高度与 height 无关。

2. block 元素的高度是由其内部的文档流决定的，可以用 height 指定高度。比如

```HTML
<div class="parent">
    <span>span标签</span>
    <div class="demo1">div标签</div>
    <div class="demo2">div标签</div>
</div>
```

那么.parent 的高都是由其内部的所有 span/div 标签的内容共同决定的。

3. inline-block 元素的高度和 block 元素的高度一致，特点也一样。

- 如果一个 div 标签里没有内容，那么它的 height 就为 0.

## overflow

当内容超出容器是，可以用 overflow 控制溢出部分的内容。

```CSS
overflow: visible 显示溢出部分
overflow: hidden  隐藏溢出部分
overflow: scroll  溢出部分有滚动条
overflow: auto    灵活设置溢出部分(该属性用的最多)
```

## 如何脱离文档流

1. 对某个元素加上 float:left/right;
2. 对某个元素进行相对/绝对定位，也即

```CSS
position: absolute/relative;
```

# CSS 盒模型(Border Box)

详情见我的另一个[博客](http://carminepink.xyz/post/css%E7%9B%92%E6%A8%A1%E5%9E%8B/)
