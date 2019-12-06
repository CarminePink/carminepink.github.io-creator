---
title: "《DOM编程》"
date: 2019-10-31T21:24:07+08:00
draft: false
---

## 什么是 DOM

- JS 用 document 操纵网页，这就是 Document Object Model 文档对象模型，简称 DOM。

## 常见的 DOM 操作 API

### 获取元素的 API

首先我们要明白，元素也叫标签，叫法不同而已。

1. `window.idxxx` 或者直接 `idxxx` 可以获取到 id 为 xxx 的元素

2. `document.getElementById('xxx')`

   获取 id 为 xxx 的元素

3. `document.getElementsByTagName('div')[0]`

   获取到标签名为 div 的元素组成的数组中的第一个元素

4. `document.getElementsByClassName('xxx')[0]`

   获取到 class="xxx"的元素组成的数组中的第一个元素

5. `document.querySelector('#idxxx')`

   获取到第一个 id 为 xxx 的元素

6. `document.querySelectorAll('.classxxx')[0]`

   获取到 class="xxx"的元素组成的数组中的第一个元素

举个例子

```JavaScript
document.querySelector('div>span:nth-child(2)')
//获取到第一个 div标签中第二个子标签span
```

**注意用 querySelector 时，语法要和 CSS 一样，获取 id 要用#xxx，获取 class 要用.**
尽量不要用 3、4、5 这三种方法获取元素

### 获取特点元素的 API

- 获取 html 元素

`document.documentElement`

虽然我们 html 标签包含了所有其他标签，但是我们也只是单独获取了 html 标签，并没有获取所有标签。

![其实网页就是一棵树](/images/dom-html.jpg)

- 获取 head 元素

  `document.head`

- 获取 body 元素

  `document.body`

- 获取所有元素(所有元素都放在一个数组里)

  `document.all`

  需要注意的是**`document.all`是第六个 falsy 值**

### 分清节点(Node)和元素(Element)

节点包括以下几种

- 数字 1 表示元素 (Element)，也叫标签 (Tag)
- 数字 3 表示文本 (Text)
- 数字 8 表示注释(Comment)
- 数字 9 表示文档(Document)
- 数字 11 表示文档片段(DocumentFragment)

可以用 xxx.nodeType 得到一个数字查看 xxx 属于哪种类型

### 一个普通 div 标签的 6 层原型链

![如图所示](/images/div-yuanxing.jpg)

## 节点的增删改查

### 增加节点的 API

- 创建一个标签节点,如创建一个 div 标签

  `let div1 = document.createElement('div')`

* 创建一个文本节点,如创建一个 text 文本

  `let text1 = document.createTextNode('你好')`

* 在标签里插入文本

1. `div1.innerText = '你好'` 或 `div1.textContent = '你好'`

2. `div1.appendChild(text1)`

上述创建的标签默认处于 JS 的线程中，并没有显示在界面中，必须把创建的这些标签插入到 head、body 中去，或者是插入到已经显示在页面中的标签中去，这样这些创建的标签才会显示在页面上。

举个例子

```JavaScript
let div1 = document.createElement('div')
div1.textContent = '你好'
document.body.appendChild(div1)//这样div1才会显示在页面上
```

- 关于 appendChild 有一个细节

```JavaScript
let div1 = document.createElement('div')
test1.appendChild(div1)
test2.appendChild(div1)
//最终div1会出现在哪里呢
```

div1 最终会出现在 test2 中，**一个元素不能出现在两个地方，除非复制一份**

### 删除节点的 API

- 用 xxx 的父节点删除 xxx

  `parentNode.removeChild('xxx')`

- 直接删除

  `xxx.remove()`

一个节点被删除只是被移出了 DOM 树，该节点还是可以重新回到页面中的，因为它只是被移到了内存中去，并没有被系统“干掉”

### 修改节点的 API

- 修改/写标准属性

1. 改 class: `div.className = 'xxx'`
2. 添加新的 class:`div.classList.add('xxx')`
3. 改 style:`div.style = '.......'`
4. 改 style 的一部分，比如只修改 color `div.style.color = 'red'`
5. 创建自定义属性: `div.setAttribute('data-x','test')`

   其中'data-x'是创建的属性名，'test'是属性值

6. 获取到自己创建的自定义属性:
   `div.getAttribute('data-x')` //'test'

   `div.dataset.x` // 'test'

   若创建的属性名是以 data-开头的，比如 data-xxx，我们就可以通过`dataset.xxx`获取到该属性的属性值

7. 修改自定义属性: `div.dataset.x = 'frank'`

- 读标准属性

有两种方法，但是第二种方法更保险

1. `div.xxx` xxx 为你要读的那个标准属性，比如`div.id`
2. `div.getAttribute('xxx')`,xxx 为你要读的那个属性

   如果读 a 标签的属性的话，一定要用第二种方法，因为用`a.href`和`a.getAttribute('href')`获取到值会有些许不同

- 改事件处理函数

  首先我们得知道 div.onclick 默认为 null

  若把 div.onclick 改为一个函数 fn,

  ```JavaScript
    div.onclick = function fn(x){
        console.log(this)
        console.log(x)
    }
  ```

  那么点击该 div 标签时，浏览器会`fn.call(div,event)`这样调用 fn,div 会被当做 this,event 就是点击事件的所有信息，比如坐标

* 改文本内容

1. `div.innerText = 'xxx'`
2. `div.textContent = 'xxx'`

- 改 HTML 内容

  `div.innerHTML = '<strong> 你好 </strong>'`

  注意这里的 HTML 是全部**大写**的，而且 innerHTML 里的内容不要写太多，否则容易造成卡顿。

* 改标签

  第一步: `div.innerHTML = ''` 先清空改标签

  第二步: `div.appendChild(div2)` 再追加内容

* 改爸爸

  找到想要的那个爸爸

  `newParent.appendChild(div)`

### 查节点的 API

- 查爸爸

  `node.parentNode` 或 `node.parentElement`

* 查爷爷

  `node.parentNode.parentNode`

* 查子代

  1. `node.childNodes`

  2. `node.children` 优先使用这一种

  有一个例子需要注意

  ```HTML
  <ul id="test">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  ```

  ```JavaScript
    console.log(test.childNodes.length) //打印出7

    console.log(test.children.length)  //打印出3
  ```

  用第一种方法查子代，**childNodes 会把文本节点也当做子代，所以那些空格回车合并之后也成为了一个子节点**，所以第一种方法打印出的子代长度是 7,而 children 就不包括文本节点，所以打印出的就是我们看到的三个子节点。

* 查兄弟姐妹

  `node.parentNode.children`

  这样查出来的结果是一个数组，还要排除自己才是所有的兄弟姐妹

* 查老大/老幺

  `node.firstChild/lastChild`

* 查上一个兄弟

  `node.previousSibling`

* 查下一个兄弟

  `node.nextSibling`

* 遍历一个 div 元素

  ```JavaScript
    let travel = (node,fn)=>{
        fn(node)
        while(node.children){
            for(let i = 0; i<node.children.length; i++){
                travel(node.children[i],fn)
            }
        }
    }
    travel(div,(node)=>{console.log(node)})
  ```

## Dom 操作是跨线程的

- 各线程各司其职

  1. JS 引擎不能操作界面，只能操作 JS

  2. 渲染引擎不能操作 JS，只能操作界面

  那么问题来了，JS 是如何通过`document.body.appendChild(div1)`这句代码使得界面增添了一个标签，发生了改变呢？这就要说到跨线程通信了

* 跨线程通信

  ![跨线程通信图示](/images/kxc.jpg)

  如图，当浏览器发现 body 增加了一个 div1 对象时，浏览器就会通知渲染引擎新增一个 div 元素，这个 div 元素的所有属性照抄 div1 对象，跨线程通信之所以慢，就是因为时间都浪费在浏览器作为"中介"通知渲染引擎做出改变。

  如果连续对 div1 对象进行操作，浏览器可能会把这些操作合并成一次操作，也可能不会。

* 属性同步

  什么是属性同步呢?在 JS 线程中对 div1 对象属性修改时，并不是所有修改都会同步到渲染引擎中，只有对**标准属性和 data-xxx 属性的修改会被同步到渲染引擎中去**，所有对**非标准属性的修改只会停留在 JS 线程中**，不会同步到渲染引擎中。

  所以如果你创建了一个自定义属性，请使用 data-作为前缀

  ![属性同步图示](/images/shuxingtongbu.jpg)

* property 与 attribute 的异同

  1. property 与 attribute 都是属性，前者代表 JS 线程中的属性，后者是渲染引擎中的属性

  2. 大部分时候，property 与 attribute 的值相等，但若不是标准属性，则两者只在一开始的时候相等，因为非标准属性的修改不会被同步到渲染引擎中去

  3. attribute 只支持字符串，而 property 支持字符串、bool 等类型
