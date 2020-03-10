---
title: "《浅析Vue修饰符sync》"
date: 2020-03-01T15:46:26+08:00
draft: false
---

## 什么是sync修饰符？什么时候要用到sync修饰符？

Vue[官网](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-%E4%BF%AE%E9%A5%B0%E7%AC%A6)上说了，在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以修改父组件，且在父组件和子组件都没有明显的改动来源。

通俗点来讲就是假设一个组件里用到了prop外部属性，而这个这个组件被一个实例所引用，那么组件里的这个prop外部属性就是实例传给这个组件的，当组件想要修改这个prop外部属性的时候，实际上是需要通知实例的，告诉它我要修改你给我传的那个属性的值了，反过来也很好理解，这就是双向绑定。但是会出现一个问题，如果组件和实例都可以随意修改这个prop外部属性的话，那么其中一个修改了之后，另一个如果不知道该属性被修改了的话，就会出现很多问题，所以sync就是为这种情况而设计出来的。我们举个例子

假设实例代码如下(引入了组件Child):
```Vue
<template>
    <div>
        实例拥有的总金额为:{{TotalMoney}}

        ***<Child :ChildMoney = "TotalMoney" v-on:update:ChildMoney ="TotalMoney = $event"/>***

        ***<Child :ChildMoney.sync = TotalMoney/>***
    </div>
</template>
<script>
    import Child from './Child.vue'
    export default{
        data(){
            return {
                TotalMoney:1000
            }
        },
        components:{
            Child:Child
        }
    }
</script>
```

假设组件Child代码如下:
```Vue
<template>
  <div>
    组件从实例那里要到的钱为:{{ChildMoney}}
    <button @click = "$emit('update:ChildMoney',ChildMoney - 100)">我花了100元</button>
  </div>
</template>

<script>
export default {
  prop:['ChildMoney']
};
</script>

```

从上面的例子我们可以看出来最重要的两句代码就是

`  <button @click = "$emit('update:ChildMoney',ChildMoney - 100)">我花了100元</button>
`

和

`
    <Child :ChildMoney = "TotalMoney" v-on:update:ChildMoney ="TotalMoney = $event"/>
`

`
    <Child :ChildMoney.sync = TotalMoney/>
`

后两句代码作用是一样的，写其中一个就可以了，下面这个代码是使用了sync修饰符。组件Child使用了外部属性ChildMoney，
而实例把TotalMoney传给组件Child的外部属性ChildMoney，当组件点击按钮修改ChildMoney的值时，会触发事件'update:ChildMoney'，组件是通过$emit来触发事件的，$emit第一个参数是触发事件的事件名，第二个参数就是传的参。
然后实例通过引用组件并且给组件传外部属性的时候来监测这些变化，$event可以获取到$emit传的参数。使用sync修饰符的话很简单，理解成即给组件传了外部属性的值，同时也监听了这个外部属性，当外部属性变化时，实例能实时接收到这个信息并做出改变。