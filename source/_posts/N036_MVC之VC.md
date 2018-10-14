---
title: MVC之VC
date: 2017-12-08 15:43:46
tags:  前端知识点
---

## 项目引入链接如下

[基础版代码](https://sltrust.github.io/cv/017mycv_show_animate.html)

[引入轮播后](https://sltrust.github.io/cv/020mycv.html)

[模块化之后](https://sltrust.github.io/cv/021mycv.html)

[立即执行函数的使用避免全局污染](https://sltrust.github.io/cv/022mycv_mvc.html)

[模块间通信与闭包的使用](https://sltrust.github.io/cv/023mycv_mvc.html)

[初步提取view和controller](https://sltrust.github.io/cv/024mycv_mvc.html)

[进阶后的vc](https://sltrust.github.io/cv/025mycv_mvc.html)



### 引入轮播

> 在啥都不清楚下引入 swiper 

![](https://raw.githubusercontent.com/slTrust/note/master/img/note036_1.png)

[swiper插件](http://idangero.us/swiper/demos/)

#### get Started

1. 安装
```
npm install swiper //安装这个swiper 或者直接用cdn的链接
```

2. 引入相关文件

```
<!DOCTYPE html>
<html lang="en">
<head>
    ...
    <link rel="stylesheet" href="path/to/swiper.min.css">
</head>
<body>
    ...
    <script src="path/to/swiper.min.js"></script>
</body>
</html>
```

3. 添加页面模板html
```
<!-- Slider main container -->
<div class="swiper-container">
    <!-- Additional required wrapper -->
    <div class="swiper-wrapper">
        <!-- Slides -->
        <div class="swiper-slide">Slide 1</div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
        ...
    </div>
    <!-- If we need pagination -->
    <div class="swiper-pagination"></div>
 
    <!-- If we need navigation buttons -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
 
    <!-- If we need scrollbar -->
    <div class="swiper-scrollbar"></div>
</div>
```
4. 加入样式文件

```
.swiper-container {
    width: 600px;
    height: 300px;
}
```

5. 初始化JS代码

```
<body>
<script>
var mySwiper = new Swiper ('.swiper-container', {
    // Optional parameters
    direction: 'vertical',
    loop: true,

    // If we need pagination
    pagination: {
        el: '.swiper-pagination',
    },

    // Navigation arrows
    navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
    },

    // And if we need scrollbar
    scrollbar: {
        el: '.swiper-scrollbar',
    }
})
</script>
</body>
```

#### 美化你的作品集 肯定不能直接截图你的作品除非很好看

> https://unsplash.com/  去这里搜你作品的关键字 一定能找到你要的精美图片

> 注意图片的比例 如果是轮播要一致

> 不要吝惜你原来无用的代码直接删掉

> 不断的调修磨你的作品集样式 调到你觉得满意为止

[代码链接](https://sltrust.github.io/cv/020mycv.html)

#### 模块化



1. 将style里的CSS放到一个地方 main.css

2. 将如下的各个功能独立成对应的js模块

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.1.0/js/swiper.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/tween.js/17.1.1/Tween.min.js"></script>
<script>
     var mySwiper = new Swiper ('.swiper-container', {
        // Optional parameters
        //direction: 'vertical',
        loop: true,

        // If we need pagination
        pagination: {
        el: '.swiper-pagination',
        },

        // Navigation arrows
        navigation: {
        nextEl: '.swiper-button-next',
        prevEl: '.swiper-button-prev',
        }
    })
</script>
<script>
    siteWelcome.classList.remove('active');
    // setTimeout(function(){
    //     siteWelcome.classList.remove('active');
    // },100);

    //给每个模块添加偏移距离的样式    然后当页面滚到对应模块时 去除该样式
    let specialTags = document.querySelectorAll('[data-x]');
    for(let i=0;i<specialTags.length;i++){
        specialTags[i].classList.add('offset');
    }

    setTimeout(() => {
        yyy();
    }, 0);

    window.onscroll = function(){
        //console.log(window.scrollY)
        // 添加粘性导航
        if(window.scrollY>0){
            topNavBar.classList.add('sticky');
        }else{
            topNavBar.classList.remove('sticky');
        }

        yyy();
       
    }

    function yyy(){
        //滚动到对应位置  高亮对应标签      
        let specialTags = document.querySelectorAll('[data-x]');
        //找到现在位置距离顶部最近的那个
        let minIndex = 0; 
        for(let i=1;i<specialTags.length;i++){
            //console.log(specialTags[i].offsetTop)
            if(Math.abs(specialTags[i].offsetTop -window.scrollY) < Math.abs(specialTags[minIndex].offsetTop -window.scrollY)){
                minIndex = i;
            }
        }

        //每个出现的模块添加过渡效果   该元素就是离顶部最近的模块  移除偏移的样式
        specialTags[minIndex].classList.remove('offset');
        
        //选中当前模块对应的a标签
        let id = specialTags[minIndex].id;
        let aTag = document.querySelector('a[href="#'+id+'"]');
        //找到aTag的爸爸 就是 li 给它加 active 同时去除其他li的active
        let li = aTag.parentNode;
        let aLi = li.parentNode.children;
        for(var i=0;i<aLi.length;i++){
            // aLi[i].classList.remove('active');
            //操作两个 样式
            aLi[i].classList.remove('highlight');
        }
        //li.classList.add('active');
        li.classList.add('highlight');
        // **解决bug的关键在于不要让两个不相干的动作操作同一个样式
        //此时已经有bug了  
        //  当前位置高亮了 但是 鼠标移入移出后  就不高亮了  因为移入移出和 滚动都是操作一个active
        //滚动到技能时候 虽然选中了  但是他的列表出现了
    }


    //这样找只有子菜单的li有 边框效果  ==》找所有li
    // let liTags = document.getElementsByClassName('menuTigger');
    let liTags = document.querySelectorAll('nav.menu > ul > li');
    //监听含有子菜单的topNavBar menu
    for(let i=0;i<liTags.length;i++){
        liTags[i].onmouseenter = function(e){
            let li = e.currentTarget;
            li.classList.add('active');
        }
        liTags[i].onmouseleave = function(e){
            let li = e.currentTarget;
            li.classList.remove('active');
        }
    }

    function animate(time) {
        requestAnimationFrame(animate);
        TWEEN.update(time);
    }
    requestAnimationFrame(animate);

    //监听导航菜单定位到对应位置
    let aTags = document.querySelectorAll('nav.menu > ul > li > a');
    for(let i=0;i<aTags.length;i++){
        aTags[i].onclick = function(e){
            //阻止默认行为  就是a标签的 锚点
            e.preventDefault();
            let a = e.currentTarget;
            let href = a.getAttribute('href');
            let element = document.querySelector(href);
           
            let top = element.offsetTop;
            //当前位置
            let currentTop = window.scrollY;
            //目标点
            let targetTop = top - 80;
            //总路程
            let s = targetTop - currentTop;
            //总时间   因为路程可能是负数  但是 时间不能是负的
            let t = Math.abs((s/100)*300);
            t = t>500?500:t;
            var coords = { x:currentTop}; 
            var tween = new TWEEN.Tween(coords) 
                    .to({ x: targetTop },t) 
                    .easing(TWEEN.Easing.Quadratic.Out) 
                    .onUpdate(function() { 
                        window.scrollTo(0,coords.x);
                    })
                    .start();
        }
    }
</script>
```

#### 模块化之后

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/Swiper/4.1.0/js/swiper.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/tween.js/17.1.1/Tween.min.js"></script>
<script src="./js/init-swiper.js"></script>
<script src="./js/auto-slide-up.js"></script>
<script src="./js/sticky-topbar.js"></script>
<script src="./js/smoothly-navigation.js"></script>
```

[代码链接](https://sltrust.github.io/cv/021mycv.html)

#### 立即执行函数的使用

### auto-slide-up.js

> 里有这样一个变量 specialTags  只要在其他js里重新赋值这个js里的代码就有问题了

```
let specialTags = document.querySelectorAll('[data-x]');

...

```

> 其他js文件里  specialTags = '我偏手贱改这个值' 上面就废了


### 错误示范使用花括号包裹你的代码 ==>这是错的 js里没有块级作用域

```
{
    var mySwiper = new Swiper ('.swiper-container', {
       ...
    })
}
```

### 使用函数作用域

```
function xxx(){
    模块的代码
}

xxx.call();
```

### 新的问题是  这个函数"xxx"又成了全局变量也是会被污染

> 匿名函数

```
// 这样会报错  这样会报错  这样会报错 
function(){
    模块的代码
}.call()
```

```
!function(){
  模块的代码
}.call()
```


1. 我们不想要全局变量
2. 我们要使用局部变量
3. ES5里，只有函数有局部变量
4. 于是我们声明了一个 function xxx,然后xxx.call()
5. 这个时候xxx是全局变量(全局函数)
6. 所以我们不能给这个函数名字
7. function(){}.call() 
8. 但是chrome不支持直接这样调用会报错
9. 试出来一个方法可以不报错 
   #### 推荐第一种
    - (1) !function(){}.call() 用来改变函数的返回值但是我们根本不在乎这个返回值
    - (2) (function(){}).call() 在函数外面加个()
        > 第二种有bug   

    - (3) fn31231231231231212.call() 不推荐  随机数的方式
```
    a  
    ()
    
    // 等价于  a()
    
    xxx 
    (function(){}).call() 相当于调用xxx 报错  所以不推荐
```


[立即执行函数的使用避免全局污染](https://sltrust.github.io/cv/022mycv_mvc.html)

### 闭包的使用

> ### 模块间通信  mod1想访问 mod2的变量

> js引入

```
<script src="./js3/module_1.js"></script>
<script src="./js3/module_2.js"></script>
```

#### module_1.js

```
!function(){
    var person = window.person = {
        name:'hjx'
    }
}.call()
```

#### module_2.js

```
!function(){
    var person = window.person;
    console.log(person);
}.call()
```

#### 闭包的使用


> ### module_1_1.js

```
!function(){
    var person = window.person = {
        name:'hjx',
        age:18
    }
    window.personAgeGrowUp = function(){
        person.age += 1;
        return person.age;
    }
}.call()
```

> ### module_2_2.js

```
!function(){
    var newAge = window.personAgeGrowUp();
    console.log(newAge)
}.call()
```

1. 立即执行函数使得 person 无法被外部访问
2. 闭包使得匿名函数可以操作 person
3. window.personAgeGrowUp 保存了匿名函数的地址
4. 任何地方都可以使用 window.personAgeGrowUp 
 =>任何地方都可以使用 window.personAgeGrowUp 操作person
    ，但是不能直接访问person

[代码链接](https://sltrust.github.io/cv/023mycv_mvc.html)

### MVC的V和C

![](https://raw.githubusercontent.com/slTrust/note/master/img/note036_2.png)

```
<!--作品集的轮播   这就是view -->
<div id="mySlides">
    <div class="swiper-container">
        <!-- Additional required wrapper -->
        <div class="swiper-wrapper">
            <!-- Slides -->
            <img src="./images/nav-page.jpg" class="swiper-slide" alt="">
            <img src="./images/canvas.png" class="swiper-slide" alt="">
            <img src="./images/slides.png" class="swiper-slide" alt="">
        </div>
        <!-- If we need pagination -->
        <div class="swiper-pagination"></div>
    </div>
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
</div>
```

```
!function(){
    //提取view
    var view = document.querySelector('#mySlides');
    var mySwiper = new Swiper (view.querySelector('.swiper-container'), {
            loop: true,
            pagination: {
            el: '.swiper-pagination',
        },
        navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
        }
    })
}.call()
```
> 其他的模块同样提取view 

```
!function(){
    var view = document.querySelector('#topNavBar');
    window.addEventListener('scroll',function(){
        if(window.scrollY>0){
            view.classList.add('sticky');
        }else{
            view.classList.remove('sticky');
        }
    })
}.call()
```

### 添加controller

```
!function(){
    var view = document.querySelector('#topNavBar');
    var controller = function(view){
        window.addEventListener('scroll',function(){
            if(window.scrollY>0){
                view.classList.add('sticky');
            }else{
                view.classList.remove('sticky');
            }
        })
    }
    controller.call(null,view)
}.call()
```

> 其余模块同上

[代码链接](https://sltrust.github.io/cv/024mycv_mvc.html)


#### sticky-topbar.js

> 初级版
```
!function(){
    var view = document.querySelector('#topNavBar');
    var controller = function(view){
        window.addEventListener('scroll',function(){
            if(window.scrollY>0){
                view.classList.add('sticky');
            }else{
                view.classList.remove('sticky');
            }
        })
    }
    controller.call(null,view);
}.call()
```

> 进阶一点

```
var controller = {
    view:null,
    init:function(view){
        this.view = view;
        window.addEventListener('scroll',function(){
            if(window.scrollY>0){
                view.classList.add('sticky');
            }else{
                view.classList.remove('sticky');
            }
        })
    }
}
controller.init.call(null,view);
```

> 再次进阶一点点 bindEvents里面的this有问题传递的是window

```
var controller = {
    view:null,
    init:function(view){
        this.view = view;
        this.bindEvents();
        //this.bindEvents.call(this)
    },
    bindEvents:function(){
        var view = this.view;
        window.addEventListener('scroll',function(){
            if(window.scrollY>0){
                view.classList.add('sticky');
            }else{
                view.classList.remove('sticky');
            }
        })
    }
}
controller.init.call(null,view);
```

> 再次进阶一点点2 bindEvents里面的this有问题传递的是window

```
!function(){
    var view = document.querySelector('#topNavBar');
    var controller = {
        view:null,
        init:function(view){
            this.view = view;
            this.bindEvents();
            //this.bindEvents.call(this)
        },
        bindEvents:function(){
            var view = this.view;
            window.addEventListener('scroll',function(){
                console.log(this)
                var view = this.view;
                if(window.scrollY>0){
                    this.active();
                }else{
                    this.deactive();
                }
            }).bind(this);
            //箭头函数没有this
        },
        active:function(){
            this.view.classList.add('sticky');
        },
        deactive:function(){
            view.classList.remove('sticky');
        }
    }
    controller.init(view);
}.call()
```

#### 解决this问题 使用箭头函数

> 示例代码 

```
var f = ()=> console.log(this);

f() //window
f.call({name:'hjx'}) // window
```
### 箭头函数内外this不变

```
bindEvents:function(){
    var view = this.view;
    window.addEventListener('scroll',()=>{
        if(window.scrollY>0){
            this.active();
        }else{
            this.deactive();
        }
    })
    //箭头函数没有this
}
```

[代码链接](https://sltrust.github.io/cv/025mycv_mvc.html)
