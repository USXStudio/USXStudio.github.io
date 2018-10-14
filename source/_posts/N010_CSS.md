---
title: CSS
date: 2017-10-21 22:44:20
tags: 前端知识点
---

## CSS (Cascading Style Sheets)

### CSS历史

[英文维基百科](https://en.wikipedia.org/wiki/Cascading_Style_Sheets#History)

> 凯斯kei丁(如果你实在不知道第一个咋读)注意！中文维基已经落后很久了

1. 两个人合作发明了 CSS
1994年哈肯·维姆·莱提出了CSS的最初建议。伯特·波斯（Bert Bos）当时正在设计一个叫做“Argo”的浏览器，他们决定一起合作设计CSS。

2. W3C 开始接管 CSS
1997年初，W3C内组织了专门管CSS的工作组，其负责人是克里斯·里雷。

3. CSS 2.1
1998年5月W3C发表了CSS2
CSS2.1修改了CSS2中的一些错误，删除了其中基本不被支持的内容和增加了一些已有的浏览器的扩展内容。

4. CSS 3
从 2011 年开始 CSS 被分为多个模块单独升级，统称为 CSS 3。这些模块有：
CSS 选择器 level 3
CSS 媒体查询 level 3
CSS Color level 3
更多请 google 搜索 CSS spec [你可以看看](https://www.w3.org/Style/CSS/specs.en.html)

5. CSS 4?
不好意思，没有 CSS 4，只有各个模块的 level 4(每个功能独立写文档)

> ps 文档很多 学海无涯生命有限，所以我们要先用在学，碰到了再去细究

周边工具

- LESS CSS
一种简化、功能更多的 CSS 语言 中文官网 英文官网
- SASS
一种简化、功能更多的 CSS 语言（请自行搜索中英文官网）
- PostCSS
一种 CSS 处理程序

> 建议是，先不要看周边工具，学好最朴素的 CSS，然后升级就很容易了。


### CSS 学习资源

1. Google: 关键词 MDN
2. [CSS Tricks](https://css-tricks.com/)（ 「居中」搜索 center css tricks）
3. Google: 阮一峰 css
4. 张鑫旭的 240 多篇 CSS 博客
5. [Codrops 炫酷 CSS 效果](https://tympanus.net/codrops/category/playground/)
6. CSS揭秘 (如果买书只推荐这一本,你买前端书千万别买中国人写的。。。)
7. [CSS 2.1 中文 spec](http://cndevdocs.com/)
8. [Magic of CSS 免费在线书](http://adamschwartz.co/magic-of-css/)(如果你三天8h看完直接成为CSS大神)

> 建议是：中文学习资源只看大 V 的（毕竟他们要维护形象不能瞎写），英文资源看 CSS Tricks、MDN 和 Codrops。书的话作用不大，最权威的书其实是文档。

- 如果你想快速上手，就先写小 demo 再学理论。
- 如果你想一鸣惊人，就仔细看 CSS 规范文档。

### 开始写 CSS

1. 引入 CSS 的三/四种方式
2. 从最小的东西开始入手
3. 逐渐变大
4. 学会组织 CSS（以后再说）
5. 自己写 CSS UI 库

#### 引入CSS的四种方式

1. 行间样式(内联样式)
    ```
    <p style="color:red;"></p>
    ```

2.  style标签 (内嵌样式)
    ```
    <head>
        <style>
            p{color:red;}
        </style>
    </head>
    ```

3. link引入
    ```
    <link href="mystyle.css" rel="stylesheet" type="text/css"/>
    ```
4. import导入
    ```
    <style type="text/css"> @import url(./b.css)</style>
    ```

#### li 的小圆点问题

```
li{list-style:none;}
```

#### 默认样式

> 除了div和span其他所有标签都有默认样式（css一个问题就是要清除默认样式）具体你可以搜 base.css/reset.css

#### float 一定会出问题

> 如果你非要深究为什么那你就回答我为啥 color:red;字会变红

解决 float 的bug 业界已经同一有了一个规范就是 .clearfix

![](https://raw.githubusercontent.com/slTrust/note/master/img/note010_01.png)

```
.clearfix::after{
    content:'',
    display:block;
    clear:both;
}
```

口诀

1. 子集浮动父级要清浮动(class="clearfix")
2. 同级一个浮动其他都浮动

#### 选择器 ">"加与不加的区别

直接子集

```
/* 匹配.topNavBar的儿子nav nav的儿子ul */
.topNavBar > nav > ul 
```

#### 默认字体大小 16px

工具使用

> chrome ==> F12 ==>Elements==> styles/computed

#### a标签默认样式 

text-decoration:underline

> 如果你实在不知道用法，搜索 mdn text-decoration

#### ul里li 鼠标滑过有边框的效果

```
<style>
    .clearfix::after{
      content:'';
      display:block;
      clear:both;
    }
    ul{border:1px solid red;}
    li{list-style:none;}
    li{float:left; ;}
    li:hover{border:1px solid red;}
</style>

<body>
    <ul class="clearfix">
    <li>111</li>
    <li>222</li>
    <li>333</li>
    </ul>
</body>
```

你会发现鼠标滑过的时候字在发抖

> 套路就是在li一开始就设置边框，只不过颜色是透明的

```
li{border:1px solid transparent;}
li:hover:{border:1px solid red;}
```

#### li里的a标签为什么有时候比li大

答案是因为a是 display:inline

```
a{display:block;}

<li>
    <a href="javascrip:;">关于</a>
</li>
```

#### inherit 值啥意思

inherit意思是继承的意思就是你爸爸什么样你就继承过来

```
body{color:#333;}
a{color:inherit;}
```

如果你不写a{color:inherit;} a标签有默认样式就会是蓝色的

清除a的默认样式

```
a{color:inherit;text-decoration:none;}
```

#### span之间有什么

如果你回车了不管你空了多少行，span直接都会有一个空格

```
<span>aaa</span>



<span>bbb</span>
<span>ccc</span>
<span>ddd</span>
```

你应该这样

```
<span>aaa</span><span>bbb</span>
```

#### 给你个需求

> 我需要一个 div 高30px，div 里有一行字垂直居中，字的大小为 14px，应该怎么写 CSS?

- 给 div 的样式为 font-size: 14px; line-height: 20px; padding: 5px 0;
- 给 div 的样式为 font-size: 14px; line-height: 24px; padding: 3px 0;
- 给 div 的样式为 font-size: 14px; line-height: 30px; 
                    

















