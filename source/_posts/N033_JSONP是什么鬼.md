---
title: JSONP是什么鬼
date: 2017-12-03 15:41:37
tags: 前端知识点
---

# 什么是jsonp
## 从零开始构建

```
mkdir jsonp_demo  //创建目录 jsonp_demo
cd jsonp_demo     //切换目录到 json_demo
touch server001.js
touch index001.html
```
## 版本一

> ### server001.js
```
var http = require('http')
var fs = require('fs')
var url = require('url')
var port = process.argv[2]

if(!port){
  console.log('请指定端口号好不啦？\nnode server.js 8888 这样不会吗？')
  process.exit(1)
}

var server = http.createServer(function(request, response){
  var parsedUrl = url.parse(request.url, true)
  var pathWithQuery = request.url 
  var queryString = ''
  if(pathWithQuery.indexOf('?') >= 0){ queryString = pathWithQuery.substring(pathWithQuery.indexOf('?')) }
  var path = parsedUrl.pathname
  var query = parsedUrl.query
  var method = request.method

  /******** 从这里开始看，上面不要看 ************/

  console.log('被请求后：含查询字符串的路径\n' + pathWithQuery)

  if(path=='/'){
    var string = fs.readFileSync('./index001.html','utf8');
    response.setHeader('Content-Type','text/html;charset=utf-8');
    response.write(string);
    response.end();
  }else{
    response.statusCode = 404;
    response.end();
  }

  /******** 代码结束，下面不要看 ************/
})

server.listen(port)
console.log('监听 ' + port + ' 成功\n请在浏览器输入 http://localhost:' + port)
```
> 当前目录下 新建index.html main.js style.css (main.js/style.css内容为空暂时不需要)
> #### index.html 
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="./style.css">
    <title>Document</title>
</head>
<body>
    <h5>您的账户余额是 <span id="amount">100</span> </h5>
    <button id="btn">付款1块钱</button>
    <script src="./main.js"></script>
    <script>
        btn.addEventListener('click',()=>{
            let n = amount.innerText;
            let number = parseInt(n,10);
            let newNumber = number - 1;
            amount.innerText = newNumber;
        })
    </script>
</body>
</html>
```
#### 运行 server001.js 所在目录  打开命令行运行
```
node server001.js 9999
//你会看到
监听 9999 成功
请在浏览器输入 http://localhost:9999
```
> 打开浏览器输入 http://localhost:9999

> ![运行后](https://raw.githubusercontent.com/slTrust/note/master/img/note033_0.png)

> 点击按钮后你会看到  余额变动如下

> ![点击付款](https://raw.githubusercontent.com/slTrust/note/master/img/note033_1.png)

#### 问题1 每次点击按钮都会把余额减1 但是浏览器只要刷新就会重新显示100
> 如何长久的保存用户的数据呢？不可能你的余额永远是100吧  那岂不是有花不完的钱
#### 长久的保存用户数据  保存到文件里
> 文件系统 将数据保存到服务器的文件里 这个文件专门为这个用户服务
```
touch db //创建 db 文件  

vi db //打开之后写入100
```

## 版本二

#### 修改html里写死的100  换用一个‘匹配’字符替换 ==> "&&&amount&&&"

> index002.html 
```
//将这一行
<h5>您的账户余额是 <span id="amount">100</span> </h5>
//修改为
//自己定义替换的匹配字符串 "&&&amount&&&" 
<h5>您的账户余额是 <span id="amount"> &&&amount&&&</span> </h5>
```

> server002.js 修改如下内容  从db读取 用户余额
```
 if(path=='/'){
    var string = fs.readFileSync('./index002.html','utf8');
    //读取db文件里用户余额
    var amount = fs.readFileSync('./db','utf8'); //100
    //替换 index002.html 里定义的 匹配字符 为 用户余额
    string = string.replace('&&&amount&&&',amount);
    response.setHeader('Content-Type','text/html;charset=utf-8');
    response.write(string);
    response.end();
  }
```
> 这样做了之后你会发现 一刷新 数据还是100 

#### 解决长久保存

> 我们点击付款的时候  告诉服务器 请把数据库里的余额给我

> 所以我们点击付款的时候不能单独修改本地了 要去给服务器发 post请求

> ### 为什么不是get 付款啊！这么重要的操作你敢用get?

## 版本三

#### index003.html
```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<form action="/pay" method="post">
    <input type="submit" value="付款1块钱">
</form>
```
#### server003.js
```
  if(path=='/'){
    var string = fs.readFileSync('./index003.html','utf8');
    var amount = fs.readFileSync('./db','utf8'); //100
    string = string.replace('&&&amount&&&',amount);
    response.setHeader('Content-Type','text/html;charset=utf-8');
    response.write(string);
    response.end();
  }else if(path=='/pay'&& method.toUpperCase() === 'POST'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
    fs.writeFileSync('./db',newAmount);  //更新余额 
    response.write('success');
    response.end();
  }else{
    response.statusCode = 404;
    response.end();
  }

```
#### 再去开启你的服务器
```
node server003.js 9999
//在浏览器打开  http://localhost:9999
```

[http://localhost:9999](http://localhost:9999)

- 点击付款按钮
- 跳到 http://localhost:9999/pay 页面显示 success
- 点击浏览器返回   **注意：此时浏览器返回的是缓存页面  你要手动刷新一下
- 用户余额减少了

> 你去打开db文件 发现余额改变了

#### 以上就是最最简单、最最原始的数据库的展示
#### 再次修改一下server003.js里的代码 
```
else if(path=='/pay'&& method.toUpperCase() === 'POST'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
    if(Math.random()>0.5){
        fs.writeFileSync('./db',newAmount);  //更新余额 
        response.write('success');
    }else{
        response.write('fail');
    }
    
    response.end();
  }


```
### 以上是：旧时代的前后端配合
> 操作数据库成功返回success 失败返回fail

### 在一个2018年的今天

> 用户怎么可能忍受
- 点击付款 
- 进入成功/失败页面
- 点击返回
- 点击刷新才看到你最新的数据

### form表单一旦提交不管成功还是失败一定会刷新当前页面

### 古老方式-优化一下用户体验  

>用iframe承载这个form的提交 后的 success/fail响应  但是I现在已经没人用啦！

## 版本四

#### index004.html
```
<body>
    <h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
    <form action="/pay" method="post" target="result">
        <input type="submit" value="付款1块钱">
    </form>
    <iframe name="result" src="about:blank" frameborder="0"></iframe>
</body>
```

#### 前端的高度洁癖 不用form向后台发送一个请求

## 版本五

#### index005.html
```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    btn.addEventListener('click',function(){
        let img = document.createElement('img');
        img.src = '/pay';
    })
</script>
```
> 点击按钮的时候 会创建一个img标签 设置它的 src = '你的请求路径' 就会触发请求

#### 这样的缺陷就是 它无法post 它只能get

#### 问题来了？ 我们如何知道它成功or失败呢？

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    btn.addEventListener('click',function(){
        let img = document.createElement('img');
        img.src = '/pay';
        //我们如何知道请求成功or失败呢？ 
        img.onload = function(){
            alert('打钱成功')
            //我们帮用户刷新  但是会重绘整个页面
            //window.location.reload();
            //因为知道固定减1 所以我帮用户减1
            amount.innerText = amount.innerText - 1;
        }
        img.onerror = function(){
            alert('打钱失败')
        }
    })
</script>
```

#### server005.js

```
else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
    if(Math.random()>0.5){
        fs.writeFileSync('./db',newAmount);  //更新余额
        //设置响应头 
        response.setHeader('Content-Type','image/png');
        //设置状态码
        response.statusCode = 200;
        //返回响应的图片  不然 会走入img.onerror 事件里
        response.write(fs.readFileSync('./dog.jpg'));
    }else{
        response.statusCode = 400;
        response.write('fail');
    }
  
    response.end();
}
```
#### 这样我们就完成了 无刷新的发送请求  俗称 image发请求法

### 改良 script发请求法

> #### 注意动态创建script 同时设置src 必须把script添加到body里才会生效

## 版本六

#### server006.js

```
  /******** 从这里开始看，上面不要看 ************/

  console.log('被请求后：含查询字符串的路径\n' + pathWithQuery)

  if(path=='/'){
    var string = fs.readFileSync('./index006.html','utf8');
    var amount = fs.readFileSync('./db','utf8'); //100
    string = string.replace('&&&amount&&&',amount);
    response.setHeader('Content-Type','text/html;charset=utf-8');
    response.write(string);
    response.end();
  }else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
    if(Math.random()>0.5){
        fs.writeFileSync('./db',newAmount);  //更新余额
        //设置响应头 
        response.setHeader('Content-Type','application/javascript');
        //设置状态码
        response.statusCode = 200;
        //返回响应内容
        response.write('');
    }else{
        response.statusCode = 400;
        response.write('fail');
    }
   
    response.end();
  }else{
    response.statusCode = 404;
    response.end();
  }

  /******** 代码结束，下面不要看 ************/

```

#### index006.html

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    btn.addEventListener('click',function(){
        let script = document.createElement('script');
        script.src = '/pay';
        document.body.appendChild(script);
        //我们如何知道请求成功or失败呢？ 
        script.onload = function(){
            alert('打钱成功')
        }
        script.onerror = function(){
            alert('打钱失败')
        }
    })
</script>
```

> 这样每次点击会生成一个script 

#### 我们知道 script里的代码是会执行的 

> 所以我们就不需要在 server006.js里写失败的情况 它会自动进入 script.onerror里

#### step1修改server006.js
```
else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
 
    fs.writeFileSync('./db',newAmount);  //更新余额
    //设置响应头 
    response.setHeader('Content-Type','application/javascript');
    //设置状态码
    response.statusCode = 200;
    //返回响应内容
    response.write('alert("pay")');
   
    response.end();
  }
```

> 运行发现如果成功了 就会 弹出pay  然后再弹出‘打钱成功’
#### 不用监听script.onload了 
> 因为 成功自然会在server006.js 里 写入并执行 response.write('alert("pay")');

## 版本七

#### index007.html
```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    btn.addEventListener('click',function(){
        let script = document.createElement('script');
        script.src = '/pay';
        document.body.appendChild(script);
        script.onerror = function(){
            alert('打钱失败')
        }
    })
</script>
```

#### server007.js

```
else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
 
    fs.writeFileSync('./db',newAmount);  //更新余额
    //设置响应头 
    response.setHeader('Content-Type','application/javascript');
    //设置状态码
    response.statusCode = 200;
    //返回响应内容
    response.write(
      `
      amount.innerText = amount.innerText - 1;
      `
    );
   
    response.end();
```

#### 优化上面运行后每次点击按钮页面都会多一个script标签 

> 成功或者失败后干掉script

## 版本八

#### index008.html  

> 注意:server里文件路径

> 所以我们还是得监听onload事件

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    btn.addEventListener('click',function(){
        let script = document.createElement('script');
        script.src = '/pay';
        document.body.appendChild(script);
        script.onload = function(e){
            alert('打钱成功')
            e.currentTarget.remove();
        }
        script.onerror = function(e){
            alert('打钱失败')
            e.currentTarget.remove();
        }
    })
</script>
```

> #### 注意：不管成功还是失败script都会移除 但是script还在内存里只是从页面移除了

#### 这个方案就叫 SRJ  (Server rendered javascript)服务器返回的 javascript

> 无刷新局部更新页面内容的方案 在ajax之前

### script是不受域名限制的 这跟跨域没关系

> aa.com:9999的前端 是可以访问  bb.com:8888的后台接口的 这就是SRJ

### JSONP

#### 查看后端代码 发现-- 后端要对前端代码非常了解
```
else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
 
    fs.writeFileSync('./db',newAmount);  //更新余额
    //设置响应头 
    response.setHeader('Content-Type','application/javascript');
    //设置状态码
    response.statusCode = 200;
    //返回响应内容
    response.write(
      `
      amount.innerText = amount.innerText - 1;
      `
    );
   
    response.end();

```

> bb.com:8888的后端居然要对 aa.com:9999的页面如此清楚

#### 耦合

#### 解耦

## 版本九

#### index009.html

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    //某域名下的前端  bb.com:9999的前端
    window.xxx = function(result){
        alert(`得到了结果是${result}`);
    }

    btn.addEventListener('click',function(){
        let script = document.createElement('script');
        script.src = '/pay';
        document.body.appendChild(script);
        script.onload = function(e){
            e.currentTarget.remove();
        }
        script.onerror = function(e){
            alert('打钱失败')
            e.currentTarget.remove();
        }
    })
    </script>
```

#### server009.js

```
else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
 
    fs.writeFileSync('./db',newAmount);  //更新余额
    //设置响应头 
    response.setHeader('Content-Type','application/javascript');
    //设置状态码
    response.statusCode = 200;
    //返回响应内容
    response.write(
      `
      xxx.call(undefined,'success') //执行xxx方法并返回  字符串
      `
    );
   
    response.end();
  }
```

### 这样的好处是什么  

- 后端不需要知道任何细节，只调用xxx.call()返回结果
- 这样两个网站之间就可以无缝的沟通

#### xxx.call()是不是很丑？ 

- 于是传递一个参数callback=xxx 
- callback ='你自己定义的方法名'

## 版本十

#### index010.html

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    //某域名下的前端  bb.com:9999的前端
    window.yyy = function(result){
        alert(`得到了结果是${result}`);
    }

    btn.addEventListener('click',function(){
        let script = document.createElement('script');
        //传递自己定义的方法名
        script.src = '/pay?callback=yyy';
        document.body.appendChild(script);
        script.onload = function(e){
            e.currentTarget.remove();
        }
        script.onerror = function(e){
            alert('打钱失败')
            e.currentTarget.remove();
        }
    })
</script>
```

#### server010.js

```
else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
 
    fs.writeFileSync('./db',newAmount);  //更新余额
    //设置响应头 
    response.setHeader('Content-Type','application/javascript');
    //设置状态码
    response.statusCode = 200;
    //返回响应内容
    response.write(
      `
      ${query.callback}.call(undefined,'success')
      `
    );
   
    response.end();
  }
```

#### JSONP解决的问题是什么  两个网站之间如何交流

- 因为script是不受域名限制的 ajax是受域名限制的
- 由于不受域名限制,我就告诉对方的网站 我要请求一个数据，
- 请你把数据给我 
- 你给我之后调用我的xxx 
- 然后把参数给我 xxx.call(undefined,参数)

#### 传递的参数不仅仅是字符串还可以是json

```
 response.write(
      `
      ${query.callback}.call(undefined,{
          "success":true,
          "data":99
      })
      `
    );
```
> json
```
{
    "success":true,
    "data":99
}
```

> json的左边是 左padding : ${query.callback}.call(undefined,

> json的右边是 右padding : )

> 于是乎   JSON + padding = JSONP

![jsonp](https://raw.githubusercontent.com/slTrust/note/master/img/note033_2.png)

## 版本十一

### index011.html

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script>
    btn.addEventListener('click',function(){
        let script = document.createElement('script');

        //生成无重复的方法名
        let fnName = 'hjx'+parseInt(Math.random()*100000,10);
        window[fnName] = function(result){
            //根据接收的json结果做处理
            if(result === 'success'){
                amount.innerText = amount.innerText - 1;
            }
        }
        //传递自己定义的方法名
        script.src = '/pay?callback=' + fnName;
        document.body.appendChild(script);
        script.onload = function(e){
            e.currentTarget.remove();
            //不管成功失败都要删除生成的fnName属性
            delete window[fnName];
        }
        script.onerror = function(e){
            alert('打钱失败')
            e.currentTarget.remove();
            //不管成功失败都要删除生成的fnName属性
            delete window[fnName];
        }
    })
</script>
```

### server011.js

```
var http = require('http')
var fs = require('fs')
var url = require('url')
var port = process.argv[2]

if(!port){
  console.log('请指定端口号好不啦？\nnode server.js 8888 这样不会吗？')
  process.exit(1)
}

var server = http.createServer(function(request, response){
  var parsedUrl = url.parse(request.url, true)
  var pathWithQuery = request.url 
  var queryString = ''
  if(pathWithQuery.indexOf('?') >= 0){ queryString = pathWithQuery.substring(pathWithQuery.indexOf('?')) }
  var path = parsedUrl.pathname
  var query = parsedUrl.query
  var method = request.method

  /******** 从这里开始看，上面不要看 ************/

  console.log('被请求后：含查询字符串的路径\n' + pathWithQuery)

  if(path=='/'){
    var string = fs.readFileSync('./index011.html','utf8');
    var amount = fs.readFileSync('./db','utf8'); //100
    string = string.replace('&&&amount&&&',amount);
    response.setHeader('Content-Type','text/html;charset=utf-8');
    response.write(string);
    response.end();
  }else if(path=='/style.css'){
    var string = fs.readFileSync('./style.css','utf8');
    response.setHeader('Content-Type','text/css');
    response.write(string);
    response.end();
  }else if(path=='/main.js'){
    var string = fs.readFileSync('./main.js','utf8');
    response.setHeader('Content-Type','text/javascript');
    response.write(string);
    response.end();
  }else if(path=='/pay'){
    //读取 db中用户余额
    var amount = fs.readFileSync('./db','utf8'); 
    var newAmount = amount - 1;  //余额减1
 
    fs.writeFileSync('./db',newAmount);  //更新余额
    //设置响应头 
    response.setHeader('Content-Type','application/javascript');
    //设置状态码
    response.statusCode = 200;
    //返回响应内容
    response.write(
      `
      ${query.callback}.call(undefined,'success')
      `
    );
   
    response.end();
  }else{
    response.statusCode = 404;
    response.end();
  }

  /******** 代码结束，下面不要看 ************/
})

server.listen(port)
console.log('监听 ' + port + ' 成功\n请在浏览器输入 http://localhost:' + port)
```

#### 写了那么多  其实你可以使用jQuery 几句话就能搞定

```
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<button id="btn">打钱</button>
<script src="//code.jquery.com/jquery-1.9.1.min.js"></script>
<script>
    btn.addEventListener('click',function(){
        $.ajax({
            url:"http://localhost:9999/pay",
            dataType:"jsonp",
            success:function(res){
                console.log(res);
            }
        })            
    })
</script>
```
#### 注意： $.ajax()只是把jsonp归纳到 jq里  而它不是ajax

### 考点1 ：请问JSONP为什么不支持POST请求

- 因为JSONP是通过动态创建script实现的
- 我们动态创建script的时候只能用get 没有办法用post

### 考点2 ： 什么是 JSONP？

- 1请求方创建一个script标签src指向响应方，并传递callback =回调函数名
- 2响应方根据callback查询参数构造一个 xxx.call(undefined,传递的内容)
- 3浏览器接到响应，就会执行 xxx.call(undefined,传递的内容)
- 4请求方得到他想要的数据