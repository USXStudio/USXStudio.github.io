---
title: jQuery_HelloWorld
date: 2017-10-10 14:11:46
tags: jQuery
---

### What is jQuery?

jQuery is a fast, small, and feature-rich JavaScript library.

#### ready()

```
$(document).ready(function() {
    //逻辑处理
});
```

> ready() 的作用是等页面的文档（document）中的节点都加载完毕后，再执行后续的代码，因为我们在执行代码的时候，可能会依赖页面的某一个元素，我们要确保这个元素真正的的被加载完毕后才能正确的使用。

#### 原生JS 和 jQ

```
window.onload = function(){
    var p = document.getElementById('#xxx');
    p.innerHTML = 'hello 我是原生方式操作dom';
    p.style.color = 'red';
}
.....
// jquery

$(document).ready(function(){
    var $p = $('#yyy');
    $p.html('hello 我是jqery方式操作dom').css('color','red');
})
```
通过jQuery方法包装后的对象，是一个类数组对象。它与DOM对象完全不同，唯一相似的是它们都能操作DOM。

#### dom对象和 jq对象相互转换

```
<div></div>
<div></div>
<div></div>
...
// 返回一个类数组对象  包含三个div的jq对象
var $div = $('div');
// 转换为原生对象
var oDiv = $div[0]
oDiv.style.color = 'red';

//也可以使用get(索引) 转换成dom对象
var oDiv2 = $div.get(1);

... dom对象转 jq对象
//getElementsByTagName()  返回的是一个集合,dom对象数组  三个div
var aDiv = document.getElementsByTagName('div');
// 转换jq对象只需要这样
var $div = $(aDiv)
$div.first().css('background','red');
```

#### id选择器

```
<div id="oDiv"></div>
//注意 id是唯一的  如果存在多个相同id的元素 ，则只返回第一个dom元素
$('#oDiv')
```

#### class选择器

```
<div class="xxx"></div>
<div class="xxx"></div>
<div class="xxx"></div>
...
//通过原生方法处理
//样式是可以多选的，所以得到的是一个合集
//需要通过循环给合集中每一个元素修改样式
var aDiv = document.getElementsByClassName('xxx');
for (var i = 0; i < aDiv.length; i++) {
    aDiv[i].style.border = "3px solid blue";
}
-------------------------------------------------
//通过jQuery直接传入class
//class选择器可以选择多个元素
$(".xxx").css("border", "3px solid red");
//$(".xxx").css()方法内部肯定是带了一个隐式的循环处理
```

#### 标签选择器

```
<div class="xxx"></div>
<div class="xxx"></div>
<div class="xxx"></div>
....

//通过原生方法处理
//获取到所有的节点标记名为div的元素
//给每一个div加上蓝色的边框
var aDiv = document.getElementsByTagName('div');
for (var i = 0; i < aDiv.length; i++) {
    aDiv[i].style.border = "3px solid blue";
}
------------------------------------
$("div").css("border", "3px solid red");
```

#### 通配符选择器

如果要获取文档中所有的元素，通过document.getElementsByTagName()中传递"*"

```
var elements1 = document.getElementsByTagName('*');
//jquery
var elements2 = $("*");
```

#### 各种选择器

```
$('parent > child') //子选择器  直接子集  父子关系  
$('parent xxx') //后代选择器  儿子 孙子 曾孙子
$('prev + next') //相邻兄弟选择器 选择紧邻在 prev元素后的 next元素
$('prev ~ siblings') //匹配prev元素后的  所有siblings元素

$(".div:first")
$(".div:last")
$(".div:even")
$(".div:odd")
$(".div:eq(2)")
$(".div:gt(3)")
$(".div:lt(4)")

...
$('input:enabled')
$('input:disabled')
$('input:checked')
$('option:selected')
```

### this选择器

```
var aaa = {
    name:"almost",
    getName:function(){
        //this,就是aaa对象
        return this.name;
    }
}
aaa.getName(); //almost
```

```
var p1 = document.getElementById('test1')
p1.addEventListener('click',function(){
    // this === p1
    //直接通过dom的方法改变颜色
    this.style.color = "red"; 
},false);

...

$('#test2').click(function(){
    //这里的this 是原生dom对象 需要转换为jquery对象才可以使用
    //通过包装成jQuery对象改变颜色
    $(this).css('color','blue');
})


```

### 属性与样式

设置属性

```
//找到第一个input，通过attr设置属性value的值
$('input').attr('value','001')
```

获取属性

```
$('input').attr('value')
```

通过function设置属性值

```
$('input').attr('value',function(i, val){
    		return '通过function设置' + val
})
```

删除属性

```
$('input').removeAttr('aaa')
```
