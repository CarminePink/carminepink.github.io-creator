---
title: "《HTML入门笔记1》"
date: 2019-09-19T14:01:54+08:00
draft: false
---

# 《HTML入门笔记1》

## HTML概述
* 1990年左右，Tim Berners-Lee（蒂姆·伯纳斯·李）发明了万维网（亦作“WWW”英文全称为“World Wide Web”），同时发明了HTML、HTTP和URL。李爵士写了第一个浏览器，写了第一个服务器，并且用自己的浏览器访问了自己的服务器
  
##HTML起手式
```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```
* 一般把    <html lang="en">这一句代码改为    <html lang="zh-CN">其他的代码都不要改。

## 章节标签
* 表示标题的h1~h6
写法为
```HTML
<h1>标题</h1>
```
* 表示章节的section标签
* 表示文章的article标签 (用的挺少的)
* 表示段落的p标签
* 表示头部的header标签
* 表示脚部的footer标签(&copy; 用来在网页上显示版权图标)
* 表示主要内容的main标签
* 表示旁支内容的aside标签
* 划分段落的div标签

## 全局属性(所有标签都有的属性)
* class
例如 
```HTML
<p class="middle border">标题</p>
```
只需要用.middle或者.border都可以选中该p标签

* contenteditable(可编辑内容)
例如
```HTML
<div contenteditable>正文</div>
```
这样用户就可以在网页上直接编辑div标签里面的内容

* style
```HTML
<body>
    <style>
      style {
        display: block;
      }
    </style>
  </body>
```
当style在body标签里时，可以用上面的代码是style标签里面内容显示在网页上
1. 当head与body中都对同一个标签设置了style样式时，以body中的样式为准。
2. 当head与body中以及JS中都对同一个标签设置了style样式时，以JS中的样式为准。


* hidden 
当某个标签里出现hidden属性时，该标签里的内容在网页上就会隐藏起来。

* id
```HTML
<p id = "xxx">正文</p>
```
1.  用#xxx就可以选中id为"xxx"的标签
2.  不到万不得已不要用id，因为当多个标签公用一个id时，并不会报错。
3.  当加了id属性后，在JavaScript中可以直接用设置样式
```JavaScript
xxx.style.border = " 1px solid red";
```
但是最好别用，因为如果设置的id="xxx"，xxx如果为window自带的全局属性的话就不行了，比如top、parent这些
```HTML
<p id = "parent">正文</p>
<p id = "top">正文</p>
```
* tabindex(设置再网页上按Tab键之后的跳转顺序)
```HTML
<div tabindex="1">正文1</div>
<div tabindex="200">正文2</div>
<div tabindex="3">正文3</div>
```
这时在网页上依次按下Tab键，跳转顺序为正文1→正文3→正文2。需要注意的是
1. 若tabindex="0"表示最后跳转到该标签
2. 若tabindex="-1"表示永远不会跳转到该标签

* title属性
(在了解title属性之前我们的先知道另一个知识)
* 当某个标签里的内容过长时，我们在网页上显示是需要省略掉过长的内容的，省略掉的内容用省略号表示，这时候就要写下面三行代码才能实现这个功能。
```HTML
 <div style="white-space: nowrap; overflow: hidden; text-overflow: ellipsis;" title="完整的内容在这里显示">超长的内容超长的内容超长的内容超长的内容超长的内容超长的内容超长的内容</div>
```
title就是用来显示你省略的内容，当你把鼠标移到省略号上就可以看到省略的内容，省略的内容就是title里写的内容。

## 内容标签
* ol+li标签(ol表示ordered list 有序列表 li表示list item)
```HTML
 <ol>
        <li>吃饭</li>
        <li>睡觉</li>
    </ol>
```
显示出来就是
1. 吃饭
2. 睡觉

* ul+li标签(ul就是 unordered list表示无序标签 li同上)
```HTML
 <ul>
        <li>上学</li>
        <li>打Dota</li>
    </ul>
```
显示出来就是
* 上学
* 打Dota

分割线<hr>
* dl+dt+dd标签(dl就是 description list,dt就是description term)
```HTML
<dl>
        <dt>小明</dt>
        <dd>今年21岁，身高181，体重138</dd>
</dl>
```
整个标签就是用来描述一个对象，dt标签里是描述的对象，dd标签里是描述的内容。

* HTML里多个空格、回车、Tab都会被合并为一个空格，要是先保留多个空格或者回车的样式，就需要用到pre标签。
* pre标签
```HTML
<div>
    <pre>正文           这里是内容</pre> 
</div>
```

* code标签(code标签里面是写代码的，常常将pre标签和code标签共同使用)
```HTML
<pre>
     <code>
        var a = 1
        console.log(a)
    </code>
</pre>
```

* a标签是超链接标签，以后会细讲。

* hr标签是分割线，写的时候只写一个<hr>即可

* br标签相当于break
```HTML
<div>
    学习计划  <br>第一天
</div>
```

* em标签和strong标签
1. em标签标示语气上的强调。
2. strong标签标示内容本身的重要性。
```HTML
<div>
    <em>这次考试</em>的重点是<strong>数据结构里的堆与栈</strong>
 </div>
```
强调这次考试，重点是数据结构的堆与栈，下次考试重点可能就不是堆与栈了。

* quote标签和blockquote标签
1. quote表示引用。
2. blockquote表示块引用。
