---
title: "《CSS动画》"
date: 2019-09-28T14:23:05+08:00
draft: false
---

# CSS 动画

首先我们的先了解浏览器的渲染过程。

## 浏览器的渲染过程

1. 根据 HTML 构建 HTML 树(DOM)
2. 根据 CSS 构建 CSS 树(CSSOM)
3. 将上面两个树合并成一棵渲染树(render tree)
4. Layout 布局(文档流、盒模型、计算大小和位置)
5. Paint 绘制(把边框颜色、背景色、文字颜色、阴影等画出来)
6. Composite 合成(根据层叠关系展示画面)

![三棵树的图解](/images/tree.jpg)

## 如何更新样式

- 一般用 JS 更新样式
  例如

```JavaScript
div.style.background = 'red'

div.style.display = 'none'

div.classList.add('end')
```

上面的代码第一句改变的是背景色，第二句改变的是显示属性，第三局增加了一个新的类，名为'end'。

- 总结一句话，添加样式不如添加一个类，因为类里面可以有多个样式。

## 三种更新方式

1. 例如，直接 `div.remove()`移除了某个元素，那么浏览器肯定要对其他所有元素进行重新的布局、绘制、和合并，所以走了一个完整的过程。
   ![第一种](/images/g1.png)

2. 例如 `div.style.background = 'red'` 仅仅只是修改了背景颜色,浏览器只需要重新绘制即可，省略了 Layout 布局这一步。
   ![第一种](/images/g2.png)

3. 例如改变了 transform，那么浏览器只会进行最后一步合并，省略了 Layout 和 Paint 两个步骤。
   ![第一种](/images/g3.jpg)

## 用 transform 制作一个动画

- transform 常用的几个属性

1. translate 移动
2. rotate 旋转
3. scale 缩放
4. skew 倾斜
5. 其他的属性 搜索 transform MDN 自己看一看
   [用 transform 制作一个跳动的心](http://js.jirengu.com/teyud/3/edit?html,css,output)
   说实话，transform 这一块讲的没啥用，自己敲一遍代码就记住这些效果了。

## transition 全解

- transition 的作用就是补充中间帧(比如从初始状态到结束状态最好有一个缓慢的变化过程，这个过程就用 transition 来描述)
- transition 语法

```CSS
  transition: 属性名(要变化的属性，比如width)| 时长| 过渡方式 |延迟
```

可以用 all 代表所有属性，比如
`transition:all 1s`

- 过渡方式有很多，linear/ease/ease-in/ease-out
  自己敲一遍看一下效果

- 并不是所有属性都可以过渡的，比如

```CSS
.container{
    display:block => none
}
```

这样就不行，因为从有到没有就是一瞬间的事情，无法中 transition 表示中间过程。

上述过程用效果也是一样的，不同的是这个属性可以用 transition 表示中间过程

```CSS
.container{
    visibility:visible => hidden
}
```

- 过渡必要要有一个起始状态
- 可以用两次 transform 实现 从 a=>transform=>b 再从 b=>transform=>c 的变化
  但是 c 一定要包含 b 的状态，否则 c 的变化是以 a 的状态为基准变化的。

## CSS 动画优化

1. JS 优化

- 使用 requestAnimationFrame 代替 setTimeout 或 setInterval

2. CSS 优化

- 使用 well-change 或 translate(transform)
  不要用`left:50px =>left: 100px`这样的代码做动画效果，要用`transform:translateX(50px);`制作动画，因为 transform 省略了 Layout 和 Paint 两个过程，这样性能就得到了优化。

## Animation 动画

这里同样有个制作跳动的红心的例子，不过这次使用 animation 实现的
[用 animation 制作一个跳动的红心](http://js.jirengu.com/waquc/1/edit?html,css,output)

animation 的语法

```CSS
animation: 动画名(关键帧名) | 时长| 过渡方式| 延迟 |次数 |方向 |填充属性 |是否暂停
```

- 想让动画效果停留在最后一帧的话，把填充属性改为 forwards 即可。

- 次数:infinite(无限次)
- 方向:reverse/alternate(左右横跳，通俗点说就是去了回，回了再去，来来回回。适合做加载动画)
- 填充模式:both/forwards
- 是否暂停:paused/running
