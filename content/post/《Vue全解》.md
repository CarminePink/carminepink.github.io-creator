---
title: "《Vue全解》"
date: 2020-02-24T16:09:20+08:00
draft: false
---

## Vue 的两种版本

Vue 有两个版本，分别是完整版和非完整版(也叫运行时版)，完整版对应引入的文件后缀为 vue.js,非完整版对应引入的文件后缀为 vue.runtime.js，当然，两者的区别不仅仅在于文件名的不同，还有很多的区别见下图。

![两种Vue版本的区别](/images/vue-qubie.jpg)

## Vue 完整版的实例

1. 我们可以把页面的视图代码直接用 html 标签写出来，这样视图就直接渲染到页面上了，而不是写在 js 中，但是要注意引入的 `script` 标签是正确的，不要弄混了，如图

![视图代码](/images/vue-wanzheng1.png)

2. 而在 js 文件可以直接对视图中的元素进行操作，如图

![js代码](/images/vue-wanzheng2.png)

## Vue 非完整版实例

1. 非完整版的视图不能直接写在 html 中，如图，`div` 标签里是空的，同时也要注意引入正确的 `script` 标签。代码如图

![如图](/images/vue-feiwanzheng1.png)

2. 那么视图的代码写在哪里呢?实际上是写在一个叫 Demo-vue 的文件里(名字随便)，里面有三个标签，分别是`template`、`script`、`style`标签，template 标签里是写视图代码，有点像 MVC 中的 V，而 script 标签里写的就是其他功能代码，就像是 MVC 中的 M 和 C。而 style 标签里写的就是样式了。代码如图

![如图](/images/vue-feiwanzheng2.png)

3. 上述代码写在 Demo-vue 文件中网页其实是看不到任何东西的，但是最终网页得看的到才行，那就需要到 js 中引入，然后写在 `render` 函数里，用变量 `h` 来创建标签,这样才能把视图渲染到页面上。当然这个过程用到了 webpack 中的 **vue-loader** 加载器。代码如图

![如图](/images/vue-feiwanzheng3.png)

- 在 codesanbox.io 上写 Vue 很简单，创建项目是它自己提供了 vue 模板，内部详细的代码自己写就行了。
