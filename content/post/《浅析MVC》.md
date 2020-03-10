---
title: "《浅析MVC》"
date: 2020-02-19T16:50:53+08:00
draft: false
---

## MVC 是什么

我们写代码为什么需要用到 MVC 设计模式呢？原因是所有页面都可以用 MVC 来优化代码结构。那么 MVC 设计模式又是什么呢？大概可以用以下四点说明什么是 MVC。

- 所有页面的代码可以写成三个对象，这三个对象分别是 M、V、C。
- M- 是指 Model(数据模型)，负责操作所有数据。
- V- 是指 View(视图)，负责所有 UI 界面。
- C- 是指 Controller(控制区)，负责其它。

举个例子，我现在做了一个模块，这个模块的功能就是有四个按钮，分别是加减乘除，点击按钮之后屏幕上的数字会做对应的算法得到新的数据。那么我们就可以把它用 MVC 来优化代码。

1. 数据相关都放到 M

```JavaScript
const m = {
  data: {
    //初始化数据
    n: parseInt(localStorage.getItem("n"))
  }
};
```

2. 视图相关都放到 V

```JavaScript
const v = {
  el: null,
  html: `
  <div>
    <div class="output">
      <span id="number">{{n}}</span>
    </div>
    <div class="actions">
      <button id="add1">+1</button>
      <button id="minus1">-1</button>
      <button id="mul2">*2</button>
      <button id="divide2">%2</button>
    </div>
  </div>`,
  init(container) {
    v.container = $(container);
    v.render();
  },
  render() {
    if (v.el === null) {
      v.el = $(v.html.replace("{{n}}", m.data.n)).prependTo(v.container);
    } else {
      const newEl = $(v.html.replace("{{n}}", m.data.n));
      v.el.replaceWith(newEl);
      v.el = newEl;
    }
  }
};
```

我们可以发现，就连原本用 Html 渲染的页面我们也可以用 JS 代码在 V 模块中渲染出来，这样整个代码就减少了耦合度，看起来很舒服，也就是说 V 模块就是负责用户看到的东西。

3. 其他相关(例如绑定事件)放到 C

```JavaScript
const c = {
  init(container) {
    v.init(container);
    c.ui = {
      //拿到重要元素
      button1: $("#add1"),
      button2: $("#minus1"),
      button3: $("#mul2"),
      button4: $("#divide2"),
      number: $("#number")
    };
    c.bindEvents();
  },
  bindEvents() {
    //这就是事件委托
    v.container.on("click", "#add1", () => {
      m.data.n += 1;
      v.render();
    });
    v.container.on("click", "#minus1", () => {
      m.data.n -= 1;
      v.render();
    });
    v.container.on("click", "#mul2", () => {
      m.data.n *= 2;
      v.render();
    });
    v.container.on("click", "#divide2", () => {
      m.data.n /= 2;
      v.render();
    });
  }
};

```

## EventBus

EventBus 主要有三个 API,分别是 on、trigger、off

```JavaScript
const eventBus = $({})
eventBus.on()
eventBus.trigger()
eventBUs.off()
```

- eventBus.on 是用来监听事件的
- eventBus.trigger 是用来触发事件的
- eventBus.off 是用来取消监听的

代码如图
![EventBus常见API](/images/eventBus.png)

我是如何理解这几个 API 的呢？举个例子，比如当数据 data 变化之后，调用 eventBus.trigger 函数发出一个信号:数据 data 变化了，然后监听了数据 data 的 eventBus.on 函数就会做出相应的操作，比如对数据重新渲染啊等等。这就是我的理解。

## 表驱动编程

由于无法 fq 查找资料，所以我根据老师讲的总结的大概知识。表驱动编程就是当有大批类似，但又不重复的代码时，我们可以将这些代码中的重要数据抽离出来，放到一个哈希表中。

举个例子，我现在做了一个模块，这个模块的功能就是有四个按钮，分别是加减乘除，点击按钮之后屏幕上的数字会做对应的算法得到新的数据，那么我应该会写四个绑定事件，分别对应四个按钮的加减乘除。而这四个绑定事件大概率是这样写的。

```JavaScript
el.on('click','#add1',()=>{
    data +=1
    ....
})
el.on('click','#minus1',()=>{
    data -=1
    ....
})
el.on('click','#mul2',()=>{
    data *=2
    ....
})
el.on('click','#div2',()=>{
    data /=2
    ....
})
```

我们会发现，这四个绑定事件函数非常相似，唯一的区别就是对不同的按钮中的数据进行了不同的操作，所以我们把关键的**按钮类名**和**对应的数据操作**拿出来放到一个哈希表里，就能简化代码了。
改成之后大概是这个样子的

![修改之后的代码](/images/biaoqudong.png)

表驱动编程的大概思想我是这样理解的，等能 fq 了我会再查查看资料更深入了解一下。

## 模块化

我的理解是:我们之前写的项目不同的功能实现可能都写在一个文件 main.js 中，而实际上不同功能模块之间的 JS 代码是没有关联的，都写在一个文件中会让代码看的很杂乱，而且不好修改。而模块化就是不同功能模块的 JS 代码写在属于自己的 JS 文件中，举个例子，比如实现第一个功能模块 app1(app1 用来管理用户登录) 的 JS 代码都写在 app1.js 文件中，最后在在一个总的 main.js 中引入 app1.js 即可，app2(app2 用来管理用户注册)，单独写在 app2.js 中，同样在 main.js 中引入，这样不同功能模块的 JS 代码都有自己的文件，一目了然，互不干扰，也便于以后的修改，这就是模块化。
