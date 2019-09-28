---
title: "《HTML常用标签》"
date: 2019-09-20T16:31:44+08:00
draft: false
---

## a 标签

首先 a 标签就是跳转到第一个地方，这个地方是很宽泛的，不仅仅是跳转到一个页面，它可以跳转如下

1. 跳转到外部页面
2. 跳转到内部锚点(通过 id 在网页上跳转到对应 id 的标签内容)
3. 跳转到邮箱或电话(发邮件/打电话)

- 属性(href/target/download/rel=noopener),
  标准写法应该是,目前就先了解 href 和 target 两个属性，download 属性不重要，rel=noopener 暂时不了解。

```HTML
<a href="" target=""></a>
```

- href 属性详细介绍

1. href 的取值可以写网址(只记住这一种写法//xxx.com)，例如

```HTML
<a href="//google.com"></a>
```

2. href 的取值还可以是路径，例如

```HTML
<a href="/a/b/c.html"></a>
```

注意这里的绝对路径里的第一个/不是指硬盘的根目录，而是 http 服务的根目录，就好比我是在/d/Jirengu/html-demo-2 中打开 VsCode 的，那么这时候我的根目录就是 html-demo-2，而不是硬盘里的 d 盘，所以它会在 html-demo-2 目录中搜索有没有 a 这个目录。

3. href 的取值还可以是伪协议，例如

```HTML
<a href="javascript:alert(1);">伪协议</a>
```

点击之后会执行 javascript:之后的代码,比如上面的点击之后页面就会弹出一个警示框，内容是 1。
现在多写成

```HTML
<a href="javascript:;">空的伪协议</a>
```

这样点击之后页面不会有任何的操作，既不会刷新，也不会跳转，就是什么都不做。

4. href 的取值还可以是邮箱(mailto:)或手机号(tel:)，例如

```HTML
<a href="mailto:hl13886803841@sina.com">发邮件</a>
<a href="tel:13886803841">打电话</a>
```

点击之后就会发邮件给指定邮箱，打电话给指定号码。

5. href 的取值还可以是 id，例如

```HTML
<a href="#xxx">点击</a>
```

在页面上点击之后会跳转到 id 为 xxx 的标签所在的内容，比如一张图片的 id 为 xxx，则会跳转到这张图。

```HTML
<a href="#">点击</a>
```

只写一个#点击之后会滚动到页面顶部。

- target 属性

1. target 的取值可以是\_blank

```HTML
<a href="//baidu.com" target="_blank">点击</a>
```

点击之后会在新的窗口打开对应的网址

2. target 的取值还可以是\_self(a 标签默认的 target 就是这个)

```HTML
<a href="//baidu.com" target="_self">点击</a>
```

点击之后会在当前窗口打开对应的网址

3. target 的取值还可以是\_top
   假设有两个页面，A 页面引用了 B 页面，B 页面中的 a 标签中的 target 属性若为 top，
   则在 B 页面中点击 a 标签之后，对应的页面会在 A 页面中打开。
   ![A页面的代码](/images/a.png)
   ![B页面的代码](/images/b.png)

4. target 的取值还可以是\_parent
   假设有三个页面，A、B、C，A 包含 B，B 包含 C，在 C 页面中 a 标签的 target="\_parent",则在 C 页面中点击 a 标签的链接后，会在 B 页面中打开新网页。

5. target 的取值还可以是程序员的命名，例如 window 的 name

```HTML
<a href="//baidu.com" target="xxx">点击</a>
```

点击之后会开一个网页名字叫 xxx 的网页，若多个 a 标签都使用了 target="xxx"，则点击链接之后都会在这个叫 xxx 的网页打开新的网页。

6. target 的取值还可以是 iframe 的 name

```HTML
<iframe src="" frameborder="0" name="yyy"></iframe>
<a href="//baidu.com" target="yyy"></a>
```

点击之后就会在名字为 yyy 的 iframe 网页中打开新页面。

- iframe 标签

```HTML
<iframe src="" frameborder="0"></iframe>
```

iframe 就是引用别的页面，src 中写的就是引用哪个页面，比如我要引用 index.html，那么就写

```HTML
<iframe src="index.html" frameborder="0"></iframe>
```

那么我当前的页面中还包含了 index.html 这个页面。

## table 标签

- table 标签中只能用三个标签，他们分别是

1. thead(表头)
2. tbody(表身)
3. tfoot(表尾)

- table 中相关的标签

1. table
2. thead
3. tbody
4. tfoot
5. tr
6. th
7. td
   注意，即使 thead 与 tfoot 标签顺序打乱了也没有影响。

- 一般表头里的内容要用 tr 和 th 标签，表身和表尾里的内容就用 tr 和 td 标签
  例如

```HTML
<table>
      <thead>
        <tr>
          <th>表头</th>
        </tr>
        <tr>
          <th>表头1</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>表身</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td>表尾</td>
        </tr>
      </tfoot>
    </table>
```

- table 中相关的样式

1. table-layout:auto/fixed
   auto 是指根据表格内容决定每一列的宽度，这里列的内容越多，这一列就越宽。
   fixed 是指根据该列首行单元格决定宽短，所有基本每一列宽度相同。

1. table-collapse:collapse
   table-spacing:0
   这两行代码是让表格的间隙合并，更美观。

## img 标签

- 作用:发出 get 请求，并展示一张图片。
- 属性(src/alt/width/height)

```HTML
<img src="1.png" alt="这里是图片加载失败后显示的内容" width=400>
```

很明显，src 里写的是图片的相对路径或者绝对路径，alt 里写的是图片加载失败后显示的内容

- 需要注意的是 img 标签里若只写了 width 或者只写了 height，那么图片会自适应缩放，不会变形。(永远不要让图片变形)

- 事件
  onload/onerror

1. onload 是图片加载成功后的响应事件，onerror 是图片加载失败后的响应事件。

```HTML
<img src="1.png" alt="" id="xxx">
    <script>
      xxx.onload = function(){
        console.log('success')
      }
      xxx.onerror = function(){
        console.log('failed')
        xxx.src='2.png'
      }
    </script>
```

当然上述写法不正确，只有测试的时候这样写。onerror 一般是在图片加载失败后进行补救的，把 src 添加另一个图片。

- 响应式(就是让图片在手机上能够适配屏幕，正常显示)
  只需要加入一句话

```HTML
<style>
  img {
    max-width: 100%;
  }
</style>
```

- 可替换元素(暂时不了解，面试可能被问 30%概率)

## form 标签

- 作用:发送 get 请求或 post 请求，然后刷新页面。
- 属性(action/method/autocomplete/target)

```HTML
<form action="/xxx" method="POST" autocomplete="on/off" target="_blank">
      <input type="text">
      <input type="submit">
</form>
```

action="/xxx"有点像 img 标签里的 src，表示向哪个页面发送请求，method 是指发送请求是用 POST 方式还是 get 方式，默认不写的话就是 get 方式。autocomplete 是自动填充，on 表示开启，off 表示关闭。打开的话，比如你向输入框输入用户名的时候，会把你的用户名自动列出来让用户选择使用哪个用户名。

```HTML
<form action="" method="POST">
      <input type="submit" value="提交">
      <button type="submit">提交</button>
    </form>
```

同样都是提交的功能，但是 input 标签和 button 标签的区别在于，input 标签内部不能再有任何其他的标签，只能有纯文本，而 button 标签内部还可以有其他标签，比如

```HTML
<button type="submit"><strong>提交</strong></button>
```

这里的 button 标签里面还有一个 strong 标签。

## input 标签

- 作用:让用户输入内容
- 属性

1. 类型 type:text/button/checkbox(复选框)/radio(单选按钮)/password/submit/file(文件) 等等
2. 其他属性:name/autofocus/checked/disabled/pattern/maxlength/value/placeholder 等
   这些属性自己敲一敲。

- 事件

1. onchange(当用户输入内容改变时触发)
2. onfocus(当用户鼠标聚焦在输入框时)
3. onblur(当用户鼠标移开输入框时)

- 验证器

```HTML
<input type="text" required>
<button>提交</button>
```

input 标签加了 required 时，用户在输入框就必须输入内容，不能为空，否则点提交是提交不了的。

## textarea 标签(多文本输入)

## select 和 option 标签(下拉选择)

```HTML
<select name="" id="">
  <option value="0">- 请选择 -</option>
  <option value="1">数学</option>
  <option value="2">英语</option>
</select>
```
