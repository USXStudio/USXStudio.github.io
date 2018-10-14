---
title: CSS伪元素和技巧
date: 2017-10-23 22:44:20
tags: 前端知识点
---

### 什么是伪元素

如果有一个div

```
<div>hi</div>
```

```
div::before{content:'「'}
dis::after{content:'」'}
```

添加如下css,你会发现生成为 「hi」，这就是伪元素，你也可以改变伪元素的display为block那样它就变成了一个块元素

### 用CSS实现各种图形

> 如果你会用google就搜索「css tricks shape」

[css tricks shape](https://css-tricks.com/examples/ShapesOfCSS/)

### 如何实现颜色渐变

> 如果你会用google就搜索「css3 linear gradient generator」

[css线性渐变生成器](http://www.colorzilla.com/gradient-editor/)

### 生成动画

> 如果你会用google就搜索「css3 animation generator」

> 可能你没搜到那就搜索「anmiation mdn」肯定有代码给你抄

### 生成阴影

> 如果你会用google就搜索「css shadow generator」

[css阴影生成器](https://www.cssmatic.com/box-shadow)


### 画出太极

如果你第一次画肯定是这样

```
#taiji{
    border-radius: 50%;
    height: 200px;
    width: 200px;
    background: linear-gradient(to bottom, rgba(255,255,255,1) 0%,rgba(255,255,255,1) 50%,rgba(0,0,0,1) 50%,rgba(0,0,0,1) 100%); 
}

#taiji .lbox,#taiji .rbox{
    float:left;
    width:100px;
    height: 100px;
    border-radius: 50%;
    background: #fff;
    margin-top:50px;
} 
#taiji .rbox{
    float:left;
    background: black;
}
#taiji .lson,#taiji .rson{
    height: 30px;
    width: 30px;
    background: #000;
    margin:35px auto;
    border-radius: 50%;
}
#taiji .rson{
    background:#fff;
}

...

<div id="taiji">
    <div class="lbox">
        <div class="lson"></div>
    </div>
    <div class="rbox">
        <div class="rson"></div>
    </div>
</div>
```

如果你喜欢深挖css

预览我做的 https://sltrust.github.io/learnCss/taiji3.html

```
#taiji{
    border-radius: 50%;
    height: 200px;
    width: 200px;
    background: linear-gradient(to bottom, rgba(255,255,255,1) 0%,rgba(255,255,255,1) 50%,rgba(0,0,0,1) 50%,rgba(0,0,0,1) 100%); 
    position: relative;
    animation-duration:3s;
    animation-name:circlearount;
    animation-iteration-count: infinite;
    animation-timing-function: linear;
}
@keyframes circlearount{
    from{
        transform: rotate(0deg);
    }
    to{
        transform: rotate(360deg);
    }
}

div#taiji::before{
    content:'';
    position: absolute;
    height:  30px;
    width: 30px;
    border-radius: 50%;
    background: #fff;
    border:35px solid #000;
    top:50px;
    left:0;
}
div#taiji::after{
    content:'';
    position: absolute;
    height: 30px;
    width: 30px;
    border-radius: 50%;
    background: #000;
    border:35px solid #fff;
    top:50px;
    right:0;
}

...


<div id="taiji"></div>
```

