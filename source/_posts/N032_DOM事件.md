---
title: DOM事件
date: 2017-12-01 15:39:47
tags: 前端知识点
---

### 事件模型应用 点击显示浮层 点击其它地方关闭浮层

> 问题1 点击按钮浮层不出现
```
<div id="wrapper" class="wrapper">
    <button id="clickMe">点我</button>
    <div id="popover" class="popover">
      浮层
    </div>
</div>
<script>
    //点击显示浮层 
    clickMe.addEventListener('click', function(){
        popover.style.display = 'block';
    })

    document.addEventListener('click', function(){
        popover.style.display = 'none'
    })
</script>
```
> 点击按钮的时候浮层确实出现了 

> 但是addEventListener('事件',fn,布尔值) 第三个参数不传代表false 代表冒泡

> 冒泡到document  

> document 发现事件队列里有 fn于是执行   popover.style.display = 'none'

### 如何解决 ==> event.stopPropagation(); 
> Prevents further propagation of the current event in the capturing and bubbling phases.

> 防止当前事件在捕获和冒泡阶段的进一步传播。

> ##### 早一点的前端肯定会认为还有一个方法阻止冒泡 event.cancelBubble = true ;不好意思 该属性已废弃

```
//点击显示浮层 
clickMe.addEventListener('click', function(event){
    popover.style.display = 'block';
    event.stopPropagation(); //这样冒泡阶段就被禁止了 
})

document.addEventListener('click', function(){
    popover.style.display = 'none'
})
```

### 问题2 虽然解决了阻止冒泡 但是当你点击浮层时  浮层也会消失

> 这样就不能在按钮上阻止冒泡 因为点击浮层也会冒泡到wrapper
- 因为btn会冒泡到 wrapper 
- popover会冒泡到 wrapper
- 所以在它们的父节点做阻止冒泡

```
<div id="wrapper" class="wrapper">
    <button id="clickMe">点我</button>
    <div id="popover" class="popover">
        浮层
    </div>
</div>
<script>
clickMe.addEventListener('click', function(e){
    popover.style.display = 'block';
})
wrapper.addEventListener('click',function(e){
    //解决点击浮层触发的冒泡
    e.stopPropagation()
})

document.addEventListener('click', function(){
    popover.style.display = 'none'
})
</script>
```

### 问题3 现在点击按钮浮层出现  点击浮层浮层浮层不消失 但是有性能问题
- 100个浮层就是100个事件队列  10000个呢？

> 现在我们先换jq来实现刚刚的效果

> ### jq提供了一个非常方便的方法  $(node).on('click',false) 第二个参数代表 阻止默认事件 和阻止传播

### 又偶生出一个bug 就是.on('事件',false) 你在checkbox外的任何一层 阻止默认事件 这个checkbox就无法选中

```
<div id="wrapper" class="wrapper">
    <button id="clickMe">点我</button>
    <div id="popover" class="popover">
        <input type="checkbox">浮层
    </div>
</div>


$(clickMe).on('click',function(){
    $(popover).show();
});
// 这样会再次出bug就是  如果浮层里有checkbox就无法选中 因为阻止了默认事件 
$(wrapper).on('click',false)  

$(document).on('click',function(){
    $(popover).hide();
})

```

> 所以我们还是单独阻止传播 

```
$(clickMe).on('click',function(){
    $(popover).show();
});

$(wrapper).on('click',function(e){
    e.stopPropagation(); //阻止冒泡
});  

$(document).on('click',function(){
    $(popover).hide();
})
```

### 解决问题3 性能问题
- 点击按钮的时候显示浮层 同时添加一个document监听
- 为什么监听不会触发 因为 wrapper哪里阻止了 冒泡
```
$(clickMe).on('click',function(){
    $(popover).show();
    //只有显示的时候监听一次 但是不会触发  因为 wrapper上阻止了冒泡
    //这样我只监听一次 click 同时马上干掉这个监听事件   
    $(document).one('click',function(){
      console.log('doc')
      $(popover).hide();
    })
});

//所以只能这样写  单独阻止它的冒泡 不阻止默认事件
$(wrapper).on('click',function(e){
    e.stopPropagation(); //阻止冒泡
});
```

### 问题4 如果不在wrapper上不阻止冒泡 点击btn为什么浮层不显示
```
$(clickMe).on('click',function(){
    $(popover).show();
    //只有显示的时候监听一次 但是不会触发  因为 wrapper上阻止了冒泡
    //这样我只监听一次 click 同时马上干掉这个监听事件   
    $(document).one('click',function(){
      console.log('doc')
      $(popover).hide();
    })
});
```
- 在你点击btn的时候它会做两件事
- 1 浮层显示出来
- 2 document的事件队列里 添加一个函数
- 3 事件开始冒泡
- 4 开始发现 document的事件队列里有一个函数
- 5 执行 document事件队列里的函数
- 6 浮层隐藏了

