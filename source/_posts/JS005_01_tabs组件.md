---
title: tabs组件
date: 2018-04-18 20:06:38
tags: JS不知深浅
---

> #### base on jQuery(基于jquery,否则请别继续)

### tabs组件

#### HTML

```
<div class="tabs">
    <ol class="tabs-bar">
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ol>
    <ol class="tabs-content">
        <li>content 1</li>
        <li>content 2</li>
        <li>content 3</li>
    </ol>
</div>
```

#### CSS

```
.tabs{}
.tabs > ol{
  list-style: none;
  margin: 0; padding: 0;
}
.tabs > ol.tabs-bar{
  display: flex;
  border-bottom: 1px solid red;
}
.tabs > ol.tabs-bar > li{
  padding: 4px 8px;
  border: 1px solid transparent;
  border-bottom: 0;
}
.tabs > ol.tabs-bar > li:hover{
  border-color: red;
}
/* 选中项显示边框 */
.tabs > ol.tabs-bar > li.active{
  border-color: blue;
}
/* 内容默认隐藏 */
.tabs > ol.tabs-content > li{
  display: none;
}
/* 选中项显示内容 */
.tabs > ol.tabs-content > li.active{
  display: block;
}
```

#### JS

```
$('.tabs').each(function(index,element) {
    $(element).children('.tabs-bar').children('li').eq(0).addClass('active')
    $(element).children('.tabs-content').children('li').eq(0).addClass('active')
})

$('.tabs').on('click', '.tabs-bar > li', function(e) {
        var $li = $(e.currentTarget)
        $li.addClass('active').siblings().removeClass('active')
        var index = $li.index()
        //从当前选中的li往外找父级元素  而不是 $('.tabs .tabs-content>li')  因为可能有多个 tabs组件
        var $content = $li.closest('.tabs').find('.tabs-content>li').eq(index)
        $content.addClass('active').siblings().removeClass('active')
})
```

但是这样只是页面上没法复用 

#### 面向对象的JS

```
function Tabs(selector) {
  this.elements = $(selector)
  this.init()
  this.bindEvents()
}

Tabs.prototype.init = function() {
  this.elements.each(function(index, element) {
    $(element).children('.tabs-bar').children('li').eq(0).addClass('active')
    $(element).children('.tabs-content').children('li').eq(0).addClass('active')
  })
}

Tabs.prototype.bindEvents = function() {
  this.elements.on('click', '.tabs-bar > li', function(e) {
    var $li = $(e.currentTarget)
    $li.addClass('active').siblings().removeClass('active')
    var index = $li.index()
    var $content = $li.closest('.tabs').find('.tabs-content>li').eq(index)
    $content.addClass('active').siblings().removeClass('active')
  })
}

var tabs = new Tabs('.tabs')
```

#### ES6写法(class)

```
class Tabs {
  constructor(selector) {
    this.elements = $(selector)
    this.init()
    this.bindEvents()
  }
  init() {
    this.elements.each(function(index, element) {
      $(element).children('.tabs-bar').children('li').eq(0).addClass('active')
      $(element).children('.tabs-content').children('li').eq(0).addClass('active')
    })
  }
  bindEvents() {
    this.elements.on('click', '.tabs-bar > li', function(e) {
      var $li = $(e.currentTarget)
      $li.addClass('active').siblings().removeClass('active')
      var index = $li.index()
      var $content = $li.closest('.tabs').find('.tabs-content>li').eq(index)
      $content.addClass('active').siblings().removeClass('active')
    })
  }
}

var tabs = new Tabs('.tabs')
```
