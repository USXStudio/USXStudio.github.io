---
title: CSS布局与定位
date: 2017-10-22 22:44:20
tags: 前端知识点
---

## CSS布局与定位

题外话如果你想要一个高清的背景图你可以去 [wallhaven](https://alpha.wallhaven.cc/)

#### div的高度由谁决定(核心知识点)

> div的高度由其内部文档流元素的高度总和决定

#### 文档流是什么(核心知识点)

> 文档内元素的流动方向

1. 如果你是一个内联元素,就是从左往右流动,如果宽度不够(流动遇到阻碍,它就换行继续从左往右)

```
<span>文字</span>
<span>文字</span>
<span>文字</span>
...
<span>文字</span>
<span>文字</span>
```

2. 如果你是一个块级元素,就是从上往下,每个块级元素另起一行

```
<div>文字</div>
<div>文字</div>
<div>文字</div>
...
<div>文字</div>
<div>文字</div>
<div>文字</div>
```

如图

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_01.png)

#### 热干面的笑话

外语认为单词的不可分割性导致 老外学中文 "热干面" 他会连续的把字向英语单词一样读出来

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_02.png)

答案是因为文化差异「haaaaaaaaaaaaaaaaa」 会被当作一个单词而不像汉字一样可以分开

> 你想分开它怎么办

```
span{word-break:break-all}
```

#### inline-block 

想要块级元素 变成一行 

> 本来可以float实现,但是很多人不会于是css3推出 inline-block

但是这是个触发很多问题的属性,会导致很多恶心的问题(建议float)

#### 内联元素的高度由什么决定(很难很难的知识点)

这要从与前端不相关的问题说起

> 回忆起你上小学学汉语拼音的时候「四线谱」

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_03.png)

> 你写汉字会中线对齐吗？ 明显不会
你写第二行的时候你会紧贴着第一行吗？明显也不会(多少都会留一些空隙表示分割)

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_04.png)


> 即使在我们知道了字有「建议行高」的存在，但是内联元素的高度仍然是不确定的

- 因为不同字体设计的建议行高不同 

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_05.png)

> #### 结论就是：多个span在一起的时候，它的高度由字体建议行高最高的那个span决定

#### 我们怎么避免内联元素这些奇怪的问题

假设需求是 span 16px 高度40px

```
<style>
    span{font-size:16px;}
    div{
        line-height:24px;
        padding:6px 0;
    }
</style>

<div>
    <span>你好</span>
</div>
```
- 在font-size比较小的时候，可以用line-height控制
- 建议设置的行高稍大于字体的大小

#### 迫不得已的 height (最容易出bug的属性)

> 初学前端的你是不是经常看到设计图之后立刻一个 height:300px;

- 一定不要写height,我们刚已经说了div的高度由其内部文档流元素的高度总和决定
- 你上来一个 height:300px; 万一div里面文档流高度301你怎么办？

- 不到情非得已千万不要写高度

#### 脱离文档流

> 请问 .main的高度是多少？ 300px

1. div1 脱离了文档流
2. .main的高度由其内部文档流元素的高度总和决定

```
<div class="main">
    <div class="div1" style="position:fixed;height:40px;">222</div>
    <div class="div2" style="height:300px"></div>
</div>
```

> 如果一个元素它是fixed定位，它的宽度就不会向外扩展

- 如果你fixed的元素原来是宽度占满的，通常要设置 width:100%;

#### width:100%(bug的根源)

- 如果你没什么事，千万不要加width:100%;

> 如果你fixed定位的元素有padding:20px; 再加上width:100%;那么它就比它爸爸的宽度还要大

div1的宽度已经比 main大了

```
<div class="main">
    <div class="div1" style="position:fixed;height:40px;padding:20px;width:100%">222</div>
    <div class="div2" style="height:300px"></div>
</div>
```

> 套路

- 移除fixed元素的padding
- 在fixed元素内部在包含一个div,让这个div有padding

```
<div class="main">
    <div class="div1" style="position:fixed;height:40px;width:100%">
        <div style="padding:20px;">222</div>
    </div>
    <div class="div2" style="height:300px"></div>
</div>
```

#### 背景图自适应

```
div{
    background-img:url(./img/aa.png);
    background-position:center center;
    background-size:cover;
}
```

#### 宽度自适应

- 不要写 width:1000px 这样你怎么拖拽网页它都是1000px

> max-width:940px; 网页宽度小于940的时候它也跟着缩小

#### div水平居中的套路

```
div{
    margin-left:auto;
    margin-right:auto;
}
```

#### 不接受宽高设置的 span

- 因为span是内联元素(display:inline)设置宽高是无效的
- 通常你要设置 display:inline-block

> 需求一个span 内容是 hello 宽70 高30你会怎么办

```
/*新手*/
span{
    display:inline-block;
    height:30px;
    width:70px;
    line-height:30px;
    text-align:center;
}

/*老司机   
1.先看不设置样式时hello会占据多少的宽高
2.经发现宽38px高22px
3.设置line-height:22px因为不同机器上字体表现的高度有所差异
*/
span{
    display:inline-block;
    padding:4px 16px;
    line-height:22px;
}
```

#### css画三角形

- border属性的使用

1. 先画个带边框的盒子
    ```
    div{
        border:10px solid red;
        height:50px;
        width:50px;
    }
    ```

2. 给四个边框四个颜色
    ```
    div{
        border:10px solid red;
        height:50px;
        width:50px;
        border-top-color:black;
        border-right-color:red;
        border-bottom-color:green;
        border-left-color:blue;
    }
    ```

3. 把div高度和宽度设置为0，四个三角出现了
    ```
    div{
        border:10px solid red;
        height:0px;
        width:0px;
        border-top-color:black;
        border-right-color:red;
        border-bottom-color:green;
        border-left-color:blue;
    }
    ```
4. 将上边框宽度设置为0
    ```
    div{
        border:10px solid red;
        height:0px;
        width:0px;
        border-top-color:black;
        border-right-color:red;
        border-bottom-color:green;
        border-left-color:blue;
        border-top-width:0px;
    }
    ```

5. 将其他方向的border颜色设置为透明
    ```
    div{
        border:10px solid transparent;
        height:0px;
        width:0px;
        border-left-color:blue;
        border-top-width:0px;
    }
    ```

#### iconfont 字体图标的使用

[iconfont](http://www.iconfont.cn/)

1. 建议你先登录一下，有好几种那种都可以

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_06.png)

2. 搜搜你需要的图标然后添加购物车

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_07.png)


3. 点击购物车,添加新项目.如xxx

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_08.png)

4. 添加项目成功后,你可以看到如下内容

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_09.png)

5. 点击查看在线链接然后生成文件,然后引入到项目中,然后点击使用帮助对着文档抄

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_10.png)

6. 以下是步骤

![](https://raw.githubusercontent.com/slTrust/note/master/img/note011_11.png)

> 以上步骤后你就可以使用 svg了

- 如何控制svg的大小 height/width
- 如何控制svg的背景色 fill

```
svg{
    height:30px;
    width:30px;
    fill:#0089e1;
}
```