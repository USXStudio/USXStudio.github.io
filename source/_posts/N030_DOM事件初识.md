---
title: DOM事件初识
date: 2017-12-01 09:00:47
tags: 前端知识点
---

### DOM事件

如果你想深挖请google搜索「dom spec」 [dom spec](https://www.w3.org/DOM/DOMTR)

- Document Object Model (DOM) Level 1 只对基本的事件进行了汇总
- Document Object Model (DOM) Level 2 增加了 Events 对事件的详细说明
- Document Object Model (DOM) Level 3 并没有增加 Events 的相关修订
- Document Object Model (DOM) Level 4 还在草案阶段

> 结论目前我们使用事件最新最广泛的标准是  dom level 2 (2000年11月3号)

#### dom 1 的一个经典问题

```
<script>
    function print(){console.log('Hi')}
</script>

<button id="x" onclick="print">A</button>
<button id="y" onclick="print()">B</button>
<button id="z" onclick="print.call()">C</button>

<script>
    x.onclick = print;
    y.onclick = print();
    z.onclick = print.call();
</script>
```

> 当把事件写在标签上的时候那个是正确调用 「B C」

```
写在标签上的事件
onclick="要执行的代码"
一旦用户点击，浏览器就eval("要执行的代码")
```

当把事件写在JS上的时候那个是正确调用 「x」

```
写在JS里的事件
onclick="函数"
一旦用户点击，浏览器就 x.onclick.call(x,函数)
```

#### dom2 事件队列

```
xxx.addEventListener('click',function(){
    console.log(2)
}) 
```

> 你肯定问怎么比 onclick更麻烦了

- onclick是唯一的
- 出现两次onclick就会发生覆盖

```
xxx.onclick = function(){
    console.log(1)
}
// 第一个onclick被覆盖了  只会打印2
xxx.onclick = function(){
    console.log(2)
}
```

> #### 事件队列的好处 eventListeners

```
xxx.addEventListener('click',function(){
    console.log(1)
}) 
xxx.addEventListener('click',function(){
    console.log(2)
}) 

当xxx被点击 打印 1 和 2 
```

#### 事件的移除

```
function f1(){
    console.log(1)
}
function f2(){
    console.log(2)
}
xxx.addEventListener('click',f1)
xxx.addEventListener('click',f2)
//移除事件队列里的 f1
xxx.removeEventListener('click',f1)

点击xxx后 打印 2
```

> 如何实现只监听one一次的效果

```
function f1(){
    console.log(1)
    //移除事件队列里的 f1
    xxx.removeEventListener('click',f1)
}
xxx.addEventListener('click',f1)
```

#### 问题1  当我点击儿子的时候，我是否点击了 爸爸和爷爷 ==》yes
```
<div id="grand1">
    爷爷
    <div id="parent1">
        爸爸
        <div id="child1">
            儿子
        </div>
    </div>
</div>

```
#### 问题2 当我点击儿子的时候，三个函数是否调用 ==> yes
```
grand1.addEventListener('click',function(){
  console.log('爷爷')
})

parent1.addEventListener('click',function(){
  console.log('爸爸')
})

child1.addEventListener('click',function(){
  console.log('儿子')
})

```

#### 问题3 三个函数的执行顺序 ==> 1 2 3 or 3 2 1 答案是都可以
> addEventListener('事件',函数,可选参数) 

> 第三个参数是 boolean值  不传代表false : 0 NaN undefined null false ''

> false的话 顺序从内到外 儿子  爸爸  爷爷
 
> true 的话 顺序从外到内 爷爷  爸爸  儿子

> ![推荐](https://raw.githubusercontent.com/slTrust/note/master/img/note032_0.png)

> 爷爷设置true 其他设置false 

```
grand1.addEventListener('click',function(){
  console.log('爷爷')
},true)

parent1.addEventListener('click',function(){
  console.log('爸爸')
})

child1.addEventListener('click',function(){
  console.log('儿子')
})
// 爷爷  儿子  爸爸
```

#### 考点 如果  儿子节点既有捕获又有冒泡执行顺序是什么
```
child1.addEventListener('click',function(){
  console.log('儿子捕获')
},true)

child1.addEventListener('click',function(){
  console.log('儿子冒泡')
},false)

// 儿子捕获
// 儿子冒泡
```

```
child1.addEventListener('click',function(){
  console.log('儿子冒泡')
},false)

child1.addEventListener('click',function(){
  console.log('儿子捕获')
},true)


// 儿子冒泡
// 儿子捕获
```
> 答案是 谁写在前面谁先触发

如果触发在元素本身上是不区分捕获和冒泡的