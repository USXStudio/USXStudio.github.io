---
title: AJAX是什么鬼
date: 2017-12-05 15:41:58
tags: 前端知识点
---

## ajax

### 请先确保你有node 或自己开一个wamp服务器否则别往下看了 浪费时间

> if(你有node)
```
安装请用
npm install  -g http-server     
卸载请用
npm uninstall  -g http-server  
```

```
mkdir demo 创建目录
cd demo    切换目录
touch index.html  创建一个html文件
```

#### 如何发请求

> form发请求

```
<form action="xxx" method="get" target="aa">
    <input type="text" name="password" value="222">
    <input type="submit" value="提交">
</form> 
<iframe src="" name="aa" frameborder="0"></iframe>
```
>
1. 开启你的服务器 
    - 如果安装了 http-server  你就在demo目录  ！ demo目录 ! demo目录 ! 打开终端 
        ```
        http-server -c-1  
        ```
    - 如果是wamp 你就开启服务器就行了 在你指定的端口 demo要放在wamp指定的www文件夹里
    
2. 把给你的url 在 chrome 里打开 
3. 请按F12 / 打开调试
4. 点击 Network
5. 点击提交按钮
6. 你会看到出现如下一行内容  请点击
![](https://raw.githubusercontent.com/slTrust/note/master/img/note034_1.png)

7. 点击之后 会看到前后台联调必不可少的几个部分

![](https://raw.githubusercontent.com/slTrust/note/master/img/note034_2.png)

8. 分别展开列表  

    > 点击view source
    
    > 点击view source
    
    > 点击view source

![](https://raw.githubusercontent.com/slTrust/note/master/img/note034_3.png)

9. get请求的四部分

![](https://raw.githubusercontent.com/slTrust/note/master/img/note034_4.png)

10. 修改form里的 method="post" 我知道你不会改的 所以看代码吧！

```
    <form action="xxx" method="post" target="aa">
        <input type="text" name="password" value="22">
        <input type="submit" value="提交">
    </form> 
    <iframe src="" name="aa" frameborder="0"></iframe>
```
   
> ![](https://raw.githubusercontent.com/slTrust/note/master/img/note034_5.png)

### form发请求

```
<form action="xxx" method="get" target="aa">
    <input type="text" name="password" value="222">
    <input type="submit" value="提交">
</form> 
<iframe src="" name="aa" frameborder="0"></iframe>
```

### a标签发请求

```
<a id="link" href="/xxx"></a>
<script>
    link.click();
</script>
```

### img发请求

```
<img src="/xxx" alt="">
```

### link发请求
```
<script>
    var link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = '/xxx';
    document.head.appendChild(link);
</script>
```

### script发请求

```
<script>
    var script = document.createElement('script');
    script.src = '/xxx';
    document.head.appendChild(script);
</script>
```

- 用 form 发请求，但是会刷新页面或新开页面
- 用 a 发 get 请求，但是也会刷新页面或新开页面
- 用 img 发 get 请求，但是只能以图片的形式展示
- 用 link 发 get 请求，但是只能以css/favicon的形式展示
- 用 script 发 get 请求，但是只能以脚本的形式运行

##### 有没有什么方式可以实现

1. get / post / put / delete 请求都行
2. 想以什么形式展示就以什么形式展示

#### 微软的突破

> IE 5 率先在 JS 中引入 ActiveX 对象（API），使得 JS 可以直接发起 HTTP 请求。
随后 Mozilla、 Safari、 Opera 也跟进（抄袭）了，取名 XMLHttpRequest，并被纳入 W3C 规范

> IE6当时全球90%的市场份额于是做了愚蠢的决定：他以为自己的在浏览器已经天下无敌了没必要那么多人去维护了就留了两个人做日常维护,把其他人都开除了，然后chrome看准了这个时机异军突起，2年就占据了40%的市场份额，现在IE是人人喊打

> ### 前端的起源就是因为IE发明了ajax

#### AJAX

> Jesse James Garrett 讲如下技术取名叫做 AJAX：async JavaScript and XML

1. 使用 XMLHttpRequest 发请求
2. 服务器返回 XML 格式的字符串
3. JS 解析 XML，并更新局部页面

#### 如何使用 XMLHttpRequest

```
myButton.addEventListener('click', (e)=>{
  let request = new XMLHttpRequest()
  request.open('get', '/xxx') // 配置request
  request.send()
  request.onreadystatechange = ()=>{
    if(request.readyState === 4){
      console.log('请求响应都完毕了')
      console.log(request.status)
      if(request.status >= 200 && request.status < 300){
        console.log('说明请求成功')
        console.log(typeof request.responseText) // 'string'
        console.log(request.responseText)
        let string = request.responseText
        // 把符合 JSON 语法的字符串
        // 转换成 JS 对应的值
        let object = window.JSON.parse(string)   
        // JSON.parse 是浏览器提供的
        console.log(typeof object)
        console.log(object)
        console.log('object.note')
        console.log(object.note)

      }else if(request.status >= 400){
        console.log('说明请求失败') 
      }

    }
  }
})
```

```
// 后端代码
if(path==='/xxx'){
    response.statusCode = 200
    response.setHeader('Content-Type', 'text/json;charset=utf-8')
    response.write(`
    {
      "note":{
        "to": "aaa",
        "from": "hjx",
        "heading": "打招呼",
        "content": "hello"
      }
    }
    `)
response.end()
```
### 注意readStaus有五种状态

- 0	open()方法还未被调用.
- 1	open()方法已经被调用.
- 2	send()方法已经被调用, 响应头和响应状态已经返回.
- 3	响应体下载中; responseText中已经获取了部分数据.
- 4	整个请求过程已经完毕.

> 如何验证状态码经过了 01234 ？ 请按以下代码顺序书写
```
let request = new XMLHttpRequest();
// open未调用
console.log(request.readyState)
request.onreadystatechange = function(){
    console.log(request.readyState)
    if(request.readyState === 4){
        if(request.status >= 200 && request.status <300){
            console.log(request.responseText);
        }else{
            console.log(request.status);
        }
    }
}
request.open('get','/xxx');
request.send();
```

### 考点:请使用原生JS实现AJAX

```
let request = new XMLHttpRequest();

request.open('get','/xxx');

request.send();

request.onreadystatechange = function(){
    if(request.readyState === 4){
        if(request.status >= 200 && request.status <300){
            console.log(request.responseText);
        }else{
            console.log(request.status);
        }
    }
}

```

### JSON 是一门语言 (抄袭的JS) 

> {name:'aa'}是对象 JS里这种方式叫做 对象字面量表达式

>{"name":"aa"} key必须是字符串 必须以 " 开头 ， 以 " 结尾

### JS VS JSON
| JS | JSON |
| - | - |
| undefined | 没有 |
| null | null |
| ['a','b'] | ["a","b"] |
| function(){} | 没有 |
| {name:'hjx'} | {"name":"hjx"} |
| 'hjx' | "hjx" |
| var a = { }; a.self = a | 搞不定没有变量 |

```
1. JSON没有抄袭 function和 undefined ，也没有抄袭symbol因为 symbol是最近出来的
2. JSON 的字符串必须是 "
3. obj.__proto__   没有原型链
```

### 请看下面的后台返回

> ### 返回的是符合JSON对象类型语法的字符串 ， 绝对不是JS的“对象” 

> 你才返回"对象" ,你全家才返回"对象"

> 后台是无法返回"对象" 给前端的 ，他只能返回字符串

```
response.write(`
    {
      "note":{
        "to": "aaa",
        "from": "hjx",
        "heading": "打招呼",
        "content": "hello"
      }
    }
```

### 同源策略

> ### 为什么form表单提交没有跨域问题，但ajax提交有跨域问题？

> 
- 因为原页面用 form 提交到另一个域名之后，原页面的脚本无法获取新页面中的内容。所以浏览器认为这是安全的。
- 而 AJAX 是可以读取响应内容的，因此浏览器不能允许你这样做。如果你细心的话你会发现，其实请求已经发送出去了，   你只是拿不到响应而已。
- 所以浏览器这个策略的本质是，一个域名的JS，在未经允许的情况下，不得读取另一个域名的内容。但浏览器并不阻止你向另一个域名发送请求。

### 只有 协议+端口+域名 一模一样才允许发 AJAX 请求

> 一模一样！一模一样！一模一样！一模一样！一模一样！一模一样！一模一样！一模一样！一模一样！

> http://12306.cn  和 http://www.12306.cn 是不同域名

#### 问题

> ### http://baidu.com 可以向 http://www.baidu.com 发AJAX请求吗  ?

> ### http://www.baidu.com 可以向 http://baidu.com 发AJAX请求吗  ?

#### 如果没有同源策略的限制 

> 任何一个网站都可以访问你支付宝的余额，QQ空间，你写的私密博客。

> 没有同源策略的浏览器就会让你的所有隐私泄露。

#### 如果 A站的前端实在想访问B站的内容

> 可以 B站后台设置一个参数 Accept-Control-Allow-Origin
```
//我允许这个域名访问
response.setHeader('Accept-Control-Allow-Origin', 'http://A.com:8001') 
```

#### 浏览器必须保证

- 协议+端口+域名 一模一样才允许发AJAX请求
- CORS 可以告诉浏览器 我俩是一家的别阻止他

### CORS 跨域资源共享

> ### Cross-Origin Resource Sharing

## 谨记

1. 你才返回"对象"，你全家才返回 "对象"
2. JS是一门语言，JSON是另一门语言，JSON 这门语言抄袭了JS
3. AJAX就是用JS发请求
4. 响应的第四部分是字符串，可以用JSON语法表示一个对象，也可以用JSON语法表示一个数组，还可以用XML语法，还可以用HTML语法，还可以用CSS语法，还可以用JS语法，还可以用你自创的语法。