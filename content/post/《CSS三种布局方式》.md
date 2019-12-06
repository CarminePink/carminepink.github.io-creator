---
title: "《CSS三种布局方式》"
date: 2019-09-27T16:39:15+08:00
draft: false
---

# CSS 布局-float 布局

- float 布局步骤

1. 给子元素加上 float:left/right;
2. 给父元素加上.clearfix (这一步非常重要)
   示例：
   HTML 中的代码如下

```HTML
<header class="clearfix">
    <div class="demo1"></div>
    <div class="demo2"></div>
</header>
```

CSS 中的代码如下

```CSS
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}

header{
  border: 1px solid black;
}
.clearfix:after{
  display:block;
  content:'';
  clear:both;
}

.demo1{
  border: 1px solid red;
  width:50px;
  height:50px;
  float:left
}
.demo2{
  border: 1px solid blue;
  width:50px;
  height:50px;
  float:left
}
```

![这是效果图](/images/float.png)
两个子元素 div 加上了 float:left 之后，它们就脱离了文档流，成为了浮动元素，这时候父元素 header 的高度就为 0 了，因为它没有内容了，所以一定要加上

```CSS
.clearfix:after{
  display:block;
  content:'';
  clear:both;
}
```

这样父元素才能包裹住他的子元素们(浮动元素)。

- 一般 float 布局是在 PC 端才会使用的，而且 float 布局不做响应式的，因为不会用在手机上。

* float 布局可以用来做两栏布局，三栏布局，四栏布局...以及平均布局

值得一提的是当用 float 布局实现平均布局的时候，通常要用到负 margin，下面是原因。
![示图](/images/margin.jpg)

1. 此时若想让一个 image 图片正好占满 imagelist，如果不用负 margin 是无法实现的，因为 margin-right:12px; 会让第四张 image 多出来 12px，这时候浏览器会自动把它排列到下一行，这样就不符合我们的预期效果。
2. 解决方法是在四个 image 外面套一个空的 div 标签，该 div 的样式加上一个

```CSS
div{
    margin-right:-12px;
}
```

这样就可以解决问题，让四个 image 完美的实现平均布局。

# CSS-flex 布局

- 在 flex 布局之前，我们的了解两个属性，一个是 container(容器)，也即父元素。另一个是 items,也即子元素。

* flex 布局更为灵活，常用作手机端页面的布局。

## 使用 flex 布局的步骤

1. 选择一个父元素，改变其 display:flex/inline-flex;这样该父元素就成了一个容器 container
2. 那么它的所有子元素就成为了 items

## container 常用的属性

1. flex-direction (改变 items 的流动方向)

```CSS
.container{
    flex-direction: cow/column/cow-reverse/column-reverse;
}
```

2. flex-wrap (改变折行,也即是否换行)

```CSS
.container{
    flex-wrap: wrap/nowrap/wrap-reverse;
}
```

wrap 为换行，nowrap 为不换行。

3. justify-content (主轴对齐方式,默认主轴是横轴，除非用 flex-direction 改变了方向。)

```CSS
.container{
    justify-content: flex-start/flex-end/center/stretch/space-between/space-around/space-stretch;
}
```

这些属性的效果自己敲一遍就都记住了。

4. align-items (次轴对齐方式,默认次轴是纵轴。)

```CSS
.container{
    align-items: flex-start/flex-end/center/stretch/baseline;
}
```

5. align-content (控制多行之间的间隔有多远)

```CSS
.container{
    align-content: flex-start/flex-end/center/stretch/space-between/space-around/space-stretch;
}
```

- flex-wrap 和 flex-direction 经常一起用，可以缩写为 flex-flow

```CSS
.container{
    flex-flow: row wrap;
}
```

第一个参数是 flex-direction 的值，第二个参数是 flex-wrap 的值。

## flex items 的属性

1. order (改变 items 的排列的先后顺序，order 越大，越靠后,order 的默认值是 0)

```CSS
.item{
    order:0
}
```

order 可以为负值，这样就会靠前显示。

1. flex-grow (用来为 item 分配剩下的多余空间，默认值是 0，0 也即不分配)

```CSS
.item{
    flex-grow: 1
}
```

flex-grow 的值越大，分配到的多余空间就越多。

3. flex-shrink (当用户缩小界面，空间不够时，用来控制如何变瘦的)

```CSS
.item{
    flex-shrink:1;  默认值
    flex-shrink:0;  为0时表示不会变瘦，铁公鸡一个
}
```

4. align-self (单独为某一个 item 设置纵向的对其方式)

```CSS
.item{
    align-self:flex-start/flex-end/center/stretch/baseline;
}
```

## 经验

- flex 布局基本可以满足所有需求。
- 如果一个块级元素的宽度用 width 指定了。

```CSS
.div{
    margin-left:auto;
    margin-right:auto;
}
```

上面这两句代码即可让其居中。
当然你也可以用下面两种方式，也即 flex 的布局让其居中。

```CSS
.container{
    justify-content:center;

    align-items:center;
}
```

# CSS-Grid 布局(二维布局)

grid 布局将会是未来的趋势，因为它的功能更加强大，更加灵活。

- grid 布局也有 container 和 items，它和 flex 布局这一方面还是很像的。

## 使用 grid 布局的步骤

和使用 flex 布局很像

1. 选择一个父元素，改变其 display:grid/inline-grid;这样该父元素就成了一个容器 container
2. 那么它的所有子元素就成为了 items

## 核心用法:使用 grid-template-areas 先把所有的区域划分出来

例如我现在想划分 5 个区域，分别是 header、aside、main、ad、footer

HTML 代码如下

```HTML
<body>
  <div class="container">
    <header>header</header>
    <aside>aside</aside>
    <main>main</main>
    <div class="ad">ad</div>
    <footer>footer</footer>
  </div>
</body>
```

CSS 代码如下

```CSS
*{
  margin:0;
  padding:0;
  box-sizing:border-box;
}
.container{
  display:grid;
  grid-gap:5px;
  grid-template-columns:50px auto 50px;
  grid-template-rows:50px 200px 50px;
  grid-template-areas:
    "header header header"
    "aside main ad"
    "footer footer footer"
}
.container>*{
  border: 1px solid blue;
}
.container>header{
  grid-area:header;
}
.container>aside{
  grid-area:aside
}
.container>main{
  grid-area:main;
}
.container>.ad{
  grid-area:ad;
}
.container>footer{
  grid-area:footer;
}
```

![最后的效果图如下](/images/grid.png)

从上面的代码和效果图可以看出，

1. 我先是在父元素 container 里面用 grid-template-areas 划分了一个三行三列的区域，
   其中第一行所有区域都叫 header，第二行第一列区域叫 aside，第二行第二列区域叫 main，第二行第三列区域叫 ad，第三行所有区域都叫 footer。
2. 然后我在对应的子元素中用 grid-area:区域名; 为其分配其中的某一块区域，这样整个布局就布置好了。

## grid-template-rows 和 grid-template-columns 以及 grid-template

1. grid-template-rows 创建/控制行

```CSS
.container{
    grid-template-rows: 40px auto 40px 40px;
}
```

创建了 4 行，高度分别是 40px、auto、40px、40px

2. grid-template-columns 创建/控制列

```CSS
.container{
    grid-template-columns: 40px auto 40px 40px;
}
```

创建了 4 列，宽度分别是 40px、auto、40px、40px

3. grid-template
   grid-template 是 grid-template-rows 和 grid-template-columns 的缩写形式。

```CSS
.container{
    grid-template: 50% 50% / 100px 100px
}
```

上面的代码表明创建了两行，每行高度占 50%，同时创建了两列，每列宽度占 100px。

## grid-gap(设置空隙)

1.设置所有元素之间的空隙

```CSS
.container{
    grid-gap:10px
}
```

2. 指定行与行或者列与列之间的空隙

```CSS
.container{
    grid-column-gap:10px;

    grid-row-gap:10px;
}
```

## grid-area(指定一块区域)

准确的说，grid-area 是 grid-row 和 grid-column 的缩写
例如

```CSS
.container{
    grid-area: 1/1/4/6;
}
```

上面的代码 第一个参数 1 表明 grid-row-start，行的第 1 根线。第二个参数 1 是 grid-column-start，列的第 1 根线。
第三个参数 4 是 grid-row-end，行的第 4 根线。最后一个 6 是 grid-column-end，列的第 6 根线。
所以整个代码就是指定了一块 3 行 5 列的区域。

- grid 适合不规则布局。
