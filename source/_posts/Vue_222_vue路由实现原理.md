---
title: Vue_222_vue路由实现原理
date: 2018-08-28 15:46:22
tags: vue
---

### vue路由实现原理

#### 第一步——前端必会之选项卡

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        x-tab{display: block;}
        x-tab>.nav>li.active{background: red;}
        x-tab>.content>li{display: none;}
        x-tab>.content>li.active{display: block;}
    </style>
</head>
<body>
    <x-tab>
        <ol class="nav">
            <li class="active">tab1</li>
            <li>tab2</li>
        </ol>
        <ul class="content">
            <li class="active">content1</li>
            <li>content2</li>
        </ul>
    </x-tab>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
    <script>
        // 路由第一步   选项卡
        $('x-tab').on('click','.nav>li',(e)=>{
            let $li = $(e.currentTarget)
            $li.addClass('active')
                .siblings().removeClass('active')
            let index = $li.index()
            $('.content>li').eq(index)
                .addClass('active')
                .siblings().removeClass('active')
        })
    </script>
</body>
</html>
```

> 传说没有那个入行的前端15分钟写不出一个选项卡，而且还是用jq。

#### 第二步——你早就知道的锚点

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        x-tab{display: block;}
        x-tab>.nav>li.active{background: red;}
        x-tab>.content>li{display: none;}
        x-tab>.content>li.active{display: block;}
    </style>
</head>
<body>
    <x-tab>
        <ol class="nav">
            <li class="active">tab1</li>
            <li>tab2</li>
        </ol>
        <ul class="content">
            <li class="active">content1</li>
            <li>content2</li>
        </ul>
    </x-tab>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
    <script>
        // 路由第二步  通过锚点
        let index = location.hash|| '#0'
        index = index.substring(1)
        $('.nav>li').eq(index)
            .addClass('active')
            .siblings().removeClass('active')
        $('.content>li').eq(index)
            .addClass('active')
            .siblings().removeClass('active')
        $('x-tab').on('click','.nav>li',(e)=>{
            let $li = $(e.currentTarget)
            $li.addClass('active')
                .siblings().removeClass('active')
            let index = $li.index()
            location.hash = index
            $('.content>li').eq(index)
                .addClass('active')
                .siblings().removeClass('active')
        })
    </script>
</body>
</html>
```

> 代码很重复是不是，往下看吧！

#### 第三步——使用锚点的优化

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        x-tab{display: block;}
        x-tab>.nav>li.active{background: red;}
        x-tab>.content>li{display: none;}
        x-tab>.content>li.active{display: block;}
    </style>
</head>
<body>
    <x-tab>
        <ol class="nav">
            <li class="active">
                <a href="#0">tab1</a>
            </li>
            <li>  <a href="#1">tab2</a></li>
        </ol>
        <ul class="content">
            <li class="active">content1</li>
            <li>content2</li>
        </ul>
    </x-tab>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
    <script>
        // 路由第二步  通过锚点
        function selectTab(){
            let index = location.hash|| '#0'
            index = index.substring(1)
            $('.nav>li').eq(index)
                .addClass('active')
                .siblings().removeClass('active')
            $('.content>li').eq(index)
                .addClass('active')
                .siblings().removeClass('active')
        }
        selectTab()
        // 路由第三步 监听锚点改变的事件
        window.onhashchange = (e)=>{
            selectTab()
        }
    </script>
</body>
</html>
```

> hash的缺陷就是很容易被篡改和影响    如一个回到顶部的锚点 #top 路由状态就改了 不是你之前的index了

#### 第四步——H5新的api——history

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        x-tab{display: block;}
        x-tab>.nav>li.active{background: red;}
        x-tab>.content>li{display: none;}
        x-tab>.content>li.active{display: block;}
    </style>
</head>
<body>
    <x-tab>
        <ol class="nav">
            <li class="active">
                <a href=./tab1>tab1</a>
            </li>
            <li>  <a href="./tab2">tab2</a></li>
        </ol>
        <ul class="content">
            <li class="active">content1</li>
            <li>content2</li>
        </ul>
    </x-tab>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
    <script>
        selectTab()
        // 路由第四步  不用锚点  用a标签的href跳转  但是我们不想跳转页面，所以就把事件阻止掉
        // 目标就是a标签点击之后不跳转
        $('x-tab').on('click','.nav > li > a',(e)=>{
            e.preventDefault()
            let a = e.currentTarget
            let aHref = a.getAttribute('href')
            console.log(aHref)
            // 最重要一步使用 History的新api pushState
            window.history.pushState({},'xxx',aHref)
            selectTab()
        })
        // 监听


        function selectTab(){
            let index = location.pathname.substring(1)|| tab1
            index = index.substring(3) - 1||0
            console.log(index)
            $('.nav>li').eq(index)
                .addClass('active')
                .siblings().removeClass('active')
            $('.content>li').eq(index)
                .addClass('active')
                .siblings().removeClass('active')
        }

    </script>
</body>
</html>
```

> 这样的方式你把外链分享出去是无法访问的必须服务器进行路径匹配。

```
// 后端代码
if(path==='/'||path==='/tab1'||path==='/tab2){
    // 这里返回同一页面就ok
}
```


