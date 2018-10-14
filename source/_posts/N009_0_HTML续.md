---
title: HTML续
date: 2017-10-18 21:02:23
tags: 前端知识点
---


### html一些标签总结

#### iframe

通常用来嵌套一个页面如百度

- 它的宽高是由自己决定的，而且你只能设置宽度100%，高度100%是没用的(CSS蛋疼之处)
- 如果你用iframe会明显感觉页面变卡了
- 它一般会出现在五年前的项目里
- frameborder="0"是去除iframe的边框的(3d边框)

```
<iframe src="http://www.baidu.com" frameborder="0"></iframe>
```

##### iframe 和 a标签一起使用时会发生什么？

- 给iframe 加上name属性  如name="xxx"
- 给a 加上target属性    如target="xxx"

```
<iframe name="xxx" src="#" frameborder="0"></iframe>
<a target="xxx" href="http://www.baidu.com">百度</a>
<a target="xxx" href="http://www.qq.com">QQ</a>
```

这样页面上你点击「百度」iframe就会显示百度的网页，点击「QQ」就会显示qq的网页

##### iframe的src属性也可以写相对路径，包含页面

前提你至少得开一个本地服务

```
<iframe src="./index2.html" frameborder="0"></iframe>
```

#### a 标签

我们分别写四个target它们都会在哪里打开呢？

建立如下文件 

index3.html

```
index3页面
<a target="_blank" href="http://www.baidu.com">百度</a>
<a target="_self" href="http://www.qq.com">QQ</a>
<a target="_parent" href="http://www.jd.com">京东</a>
<a target="_top" href="http://www.taobao.com">淘宝</a>
```

index2.html

```
index2页面
<iframe src="./index3.html" frameborder="0"></iframe>
```

index.html

```
index页面
<iframe src="./index2.html" frameborder="0"></iframe>
```

> 你可以 http-server的工具 

```
#全局安装
npm install http-server -g  
#开启本地服务 
http-server -c-1 
```

- _blank 会新开一个窗口打开
- _self 会在自己页面打开，如上结果会在index3页面打开
- _parent 会在父页面打开，如上结果会在index2页面打开
- _top 会在顶级窗口打开， 如上结果会在index页面打开

##### a标签之 download 属性

> download可以下载东西，可能是安装包可能是网页

- 如果不加download 
服务器要设置 Content-Type:application/octet-stream

- 加了 download 就可以直接下载

```
<a  href="http://www.qq.com" download>下载</a>
```

##### a标签纸 href属性

> 只写qq.com会跳转到 qq.com的网页吗？  不会

因为这是一个相对地址

> 比如你在命令行里  touch 1.txt / open 1.txt 都是相对当前命令行的目录执行的

```
<a  href="qq.com">qq</a>
```

修改 href="//qq.com"

> 代表以当前协议打开

- 如果你是一个本地文件打开是文件协议 file协议

- 如果你想打开qq.com的网页,你需要以http协议打开

那么你要下载一个 http-server 的工具，这样就能本地开一个node服务

```
#全局安装
npm install http-server -g  
#开启本地服务   -c-1代表不要缓存
http-server -c-1 
```

###### href="//qq.com"是 无协议绝对地址自动继承协议

###### href="xxx.html" 相对路径

- A /index.html/xxx.html
- B /xxx.html √

###### href="?name=hjx"

- A 保存
- B 这样很自然 √  如果你点击会发起get请求并把参数拼接到当前url上

###### href="#sss" 锚点是不发请求了

###### href="javascript:;" 伪协议

#### 一些问题

```
<a href="">link</a> 标签被点击后会发生什么？
当前页面刷新

<a href="/..">link</a> 标签被点击后会发生什么？
浏览器发起 GET / HTTP/1.1请求

```



> 我想写一个a标签,但是点击之后不要跳转

- 写#(锚点) 页面会动
- 不写href属性 a就跟span一样了
- 伪协议 href="javascript:;"





