---
title: vue组件间通信
date: 2018-04-12 23:58:22
tags: 前端知识点
---

#### vue 父子通信

> 爸爸是咋叫儿子的！

- 父组件在使用子组件时   绑定属性  :xxx="你定义的值"
- 子组件设置props属性,可以在页面上插值{{xxx}}

    ```
    {
        props:['xxx'],
        template:`<div>{{xxx}}</div> `
    }
    ```

```
<div id="app">
  <child :msg="data"></child>
</div>

Vue.component('child',{
  props:['msg'],
  template:`<div>
              <div>子组件==>{{msg}}</div>
            </div>`
})
new Vue({
  el:'#app',
  data:{
    data:'我是父组件的数据'
  }
})
```


> 儿子是怎样叫爸爸的！

- 子组件向上要通过emit('xxx'),xxx是事件名称
- 父组件使用子组件时，在子组件上监听事件 @xxx="相应处理"

```
<!-- html如下 -->
<div id="app">
  <child @close="show=false" v-show="show"></child>
</div>

//js如下
Vue.component('child',{
  template:`<div>
              <div>子组件</div>
              <button @click="$emit('close')">close</button>
            </div>`
})
new Vue({
  el:'#app',
  data:{
    msg:'父组件的数据',
    show:true
  }
})
```

#### 爷孙通信

> 如果是爷孙？

vue是不支持的，需要儿子叫爸爸，爸爸叫爷爷，这样层层传递

- 所以在vue里爷孙通信是很麻烦的
- 不过你可以使用单一事件管理组件eventHub来解决
- 这里用使用最通俗的方式，你也可以挂载在Vue原型上
    ```
    Vue.prototype.$eventHub = new Vue()
    ```

- 重新构造一个Vue对象  专门用来负责管理通信
- 在嵌套的组件内部的created()里 eventHub.$on('xxx',()=>{相关逻辑处理})
- 在子组件里eventHub.$emit('xxx')触发
```
<!-- html -->
<div id="app">
  <father></father>
</div>

....


//js部分

// 单一事件管理组件 vuex的前身
var eventHub =new Vue();

Vue.component('son',{
  template:`<div>
              <div>我是子组件
                 <button @click="callFather">发送给爸爸</button>
                 <button @click="callGrandPa">发送给爷爷</button>
              </div>
            </div>`,
  methods:{
    callFather(){
      eventHub.$emit('father')
    },
    callGrandPa(){
      eventHub.$emit('grandpa')
    }
  }
})

Vue.component('father',{
  template:`<div>
              <div>父组件div>
              <son></son>
            </div>`,
  created(){
      eventHub.$on('father',()=>{alert('儿子叫爸爸')})
  }
})

new Vue({
  el:'#app',
  data:{
    data:'我是爷爷',
   
  },
  created(){
    eventHub.$on('grandpa',()=>{alert('儿子叫爷爷')})
  }
})
```


