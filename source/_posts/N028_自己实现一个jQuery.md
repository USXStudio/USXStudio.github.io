---
title: 自己实现一个jQuery
date: 2017-11-29 15:27:42
tags: 前端知识点
---

# What is jQuery?

> 核心就是一句话 The Write Less,Do More（写更少，做更多）  ， ok ， let's go！

- addClass( )功能  （最渣版）

```
<ul>
    <li id="item1">选项1</li>
    <li id="item2">选项2</li>
    <li id="item3">选项3</li>
    <li id="item4">选项4</li>
    <li id="item5">选项5</li>
</ul>
```

> 如何给id="item3" 添加样式

```
function addClass(node,classArr){
    var len = classArr.length;
    for(let i = 0 ;i < len ; i++){
        node.classList.add(classArr[i]);
    }
}
// item3 通过id名可以直接获取元素
addClass(item3,['red']); 
// 你也可以这样
addClass(document.getElementById('item4'),['red'])
```

> 注意：通过id获取元素不是100%没问题的，你要是非 id="parent"  这样就不好使,你可以打印看看parent是啥

------------------------------------------------------------------

> 这样只是初步封装了方法,但是方法是还在全局作用域,你叫addClass 到时候别人也叫addClass 你可能就被覆盖了  我们是有版权的   来来来 来个帅气签名！

### addClass( ) (签名版)

> 命名空间：给全局对象挂载一个对象 window.myDom = { } 
然后在你挂载的myDom 对象上挂载你的属性 、方法

> 这比之前好在哪里？

> 好在有“从属”关系而不是“零散“的变量  
（至少在阿里没落前）我是阿里的前端  相比 我是一个前端 那个更有份量呢？ 

```
window.myDom = {};
myDom.addClass = function (node,classArr){
    var len = classArr.length;
    for(let i = 0 ;i < len ; i++){
        node.classList.add(classArr[i]);
    }
}
myDom.addClass(item3,['red']);
```

- 你肯定会不爽因为还不如之前用着爽了  每次还得加命名前缀
    myDom.fn(param)
    myDom.fn2(param)
    直接 item3.addClass(['red']) 才爽 有需求就会有改进

### 反正都是Node的api 我直接改原型不就好了 于是乎！你又挖了个坑

```
Node.prototype.addClass =  function (node,classArr){
    var len = classArr.length;
    for(let i = 0 ;i < len ; i++){
        this.classList.add(classArr[i]);
    }
}
 item3.addClass(['red']);  //你遂了心意了 
```

- 这样的后果是别人也这样改原型，而且也添加个addClass
 是不是心好累！  为啥？此乃优良传统  
山寨一下 改改加点“我想出"的功能 我就敢说我的比你好！ （王者荣耀 与 dota ） 
why?你用上了爱迪生的电灯泡 我就问你，你是自己发明一个还是在借鉴他？
看得远是因为我站在巨人的肩膀上！

### 我们是讲版权的我的就是我的 

#### 我自创一个Node2行不行 以后可能还有Node3

```
window.Node2 = function(node){
            return {
                addClass:function(classArr){
                    var len = classArr.length;
                    for(let i = 0 ;i < len ; i++){
                        node.classList.add(classArr[i]);
                    }
                }
            }
        }
var node2 = Node2(item4);
node2.addClass(['red']);  //完美
// Node2(item5).addClass(['red'])   是不是有种似曾相识的感觉
```

### 说好的jQuery呢？ 你把Node2替换成jQuery  在试试 

> 是不是有点感觉了   这是最简单版  以后会慢慢高大上的

### jQuery最强大的莫过于选择器 Selector

> 你肯定听说过一句话  "选择大于能力" (啊！没听过？  现在听过了！)

```
window.jQyery = function(nodeOrSelector){
            let node ; 
            if(typeof nodeOrSelector ==='string'){
                node = document.querySelector(nodeOrSelector);
            }else{
                node = nodeOrSelector;
            }
            return {
                addClass:function(classArr){
                    var len = classArr.length;
                    for(let i = 0 ;i < len ; i++){
                        node.classList.add(classArr[i]);
                    }
                }
            }
        }
var jqyery = jQyery(item4);
jqyery.addClass(['red']); 
```

### 如果用过的人肯定知道jQuery选择器返回的是个伪数组才对

> 我们使用querySelectorAll()它返回的是一个节点数组（伪数组）

> 然后给我们内部声明的 nodes 复制获取的每个节点

> 如果是节点数组就挨个拷贝  如果是一个 我也把你搞成伪数组哪怕就一个甚至没有
```
window.jQyery = function(nodeOrSelector){
            let nodes = {}; 
            if(typeof nodeOrSelector ==='string'){
                let temp = document.querySelectorAll(nodeOrSelector);
                for(let i=0;i<temp.length;i++){
                    nodes[i] = temp[i];
                }   
                nodes.length = temp.length;
            }else if(nodeOrSelector instanceof Node){
                nodes = {
                    0:nodeOrSelector,
                    length:1
                }
            }
            nodes.addClass = function(classes){
                for(let i=0;i<classes.length;i++){
                    for(let j=0;j<nodes.length;j++){
                        nodes[j].classList.add(classes[i]);
                    }
                }
                return nodes;
            } 
            return nodes;
        }
//这样我们就可以用$(选择器)调用了
window.$ = jQyery;
//$(item3).addClass(['red']);
$('ul li').addClass(['red'])
```
### 你肯定会疑惑 为啥每次都 return nodes ==> 答案就是链式操作

```
$('ul li').addClass(['red']).Text('hi'); //Text 我们还没有实现  ok 马上开始
```

> 在内部继续挂载方法   Text( ) 你传递参数就代表设置text你不传递就代表get

```
nodes.Text = function (text){
    if(text === undefined){
        var texts = [];
        for(let i=0;i<nodes.length;i++){
            texts.push(nodes[i].textContent);
        }
        return texts;
    }else{
        for(let i=0;i<nodes.length;i++){
            nodes[i].textContent = text;
        }
        return nodes;
    }
}
```

> 欢迎批评指正！ 

### END 
