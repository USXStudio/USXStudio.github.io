---
title: express三连(一)
date: 2018-04-26 23:08:17
tags: express
---

### express 

> 基于 Node.js 平台，快速、开放、极简的 web 开发框架。

[express](http://www.expressjs.com.cn/)

### express helloworld

1. 初始化项目(请确保你安装了node)

```
mkdir demo
cd demo
# 初始化 package.json 一路回车
npm init   
# 安装express 我的是4.16.3版本
npm install express --save
```

2. 创建server.js

```
var express = require('express');
var app = express();

app.get('/', function(req, res){
  res.send('hello world');
});

app.listen(8001,()=>{
    console.log('port 8001 is listening!')
});
```

3. 开启服务器  node server.js

4. 浏览器访问 localhost:8001

### 参数信息

> 请求url为 http://localhost:8001/?name=22 

```
app.get('/', function(req, res){
  //获取请求方式
  console.log(req.method)
  //请求参数为 k/v形式
  console.log(JSON.stringify(req.query));
  //根据url取参数 
  console.log(req.query.name);
  //返回数据
  return res.json({code:0})
});
```

> restful风格的匹配参数

restful风格 http://localhost:8001/aa/18/man

```
app.get('/:name/:age/:gender', function(req, res){
  console.log(req.params.name);
  console.log(req.params.age);
  console.log(req.params.gender);
  return res.json({code:0})
});
```

### 获取请求头和设置响应头

- request.get()
- response.set()

```
// 获取请求头 和设置响应头
app.get('/', function(req, res){
  console.log(req.get('Accept'));
  res.set('token','fdsalkjljjfadslkjfds')
  return res.json({code:0})
});
```

### 设置json格式的响应类型

- res.json({code:0})
- 麻烦的方式 先设置Content-Type 在去调对象序列化字符串

```
// 响应json格式
app.get('/', function(req, res){
   return res.json({code:0})
});
```

```
// 响应json格式
app.get('/', function(req, res){
    res.set('Content-Type','application/json;charset=utf-8')
    return res.send(JSON.stringify({code:0}))
});
```

### 设置状态码

```
res.status(404)
```

### get && post 

```
app.get('/', function(req, res){
   return res.json({code:0})
});

app.post('/', function(req, res){
   return res.json({code:0})
});
```

### app.use()

> 可以直接包含 get、post 等的匹配就不用同一路径分开写了 app.get('/user',[args...]) app.post('/user',[args..])

```
var express = require('express');

app.use('/',(req,res,next)=>{
    res.json({msg:"use"})
})

app.listen(8001,()=>{
    console.log('port 8001 is listening!')
});
```

### 中间件

> 类似promise里的 .then().then().then() 的加工处理

```
var express = require('express');
var app = express();

// const Router = express.Router();

app.use('/',mod1,mod2)

function mod1(req,res,next){
    console.log('进入 mod1 厂房')
    return next();
}

function mod2(req,res,next){
    console.log('进入 mod2 厂房')
    res.json({msg:'数据返回结束'})
}

app.listen(8001,()=>{
    console.log('port 8001 is listening!')
});
```

> use使用中间件的方式

```
app.use('/',mod1,mod2)
app.use('/',[mod1,mod2])
app.use('/',[mod1],mod2)
```

> 处理过程中通过req携带参数

```
app.use('/',mod1,mod2)

function mod1(req,res,next){
    console.log('进入 mod1 厂房')
    req.user = {name:"fsafsdfas"};
    return next();
}

function mod2(req,res,next){
    console.log('进入 mod2 厂房')
    console.log(JSON.stringify(req.user))
    res.json({msg:'数据返回结束'})
}
```

#### next() 控制流程 


- 不传递参数就会进入下一个中间件
- 传递Error对象 就会走进错误的处理分支
- 传递字符串 也会进入错误的处理分支 但是获取不到错误信息
- 传递字符串除非你是'route' 否则都会进入错误分支

```
next()
next(new Error('fdsfdsa'))
next('fdsafsa')
next('route')
```

#### 中间件的兜底处理 

```
var express = require('express');
var app = express();

app.use('/',mod1,mod2)

function mod1(req,res,next){
    console.log('进入 mod1 厂房')
    return next(new Error('少了一个零件，请求召回'));
}

function mod2(req,res,next){
    console.log('进入 mod2 厂房')
    res.json({msg:'数据返回结束'})
}

//兜底处理
app.use((err,req,res,next)=>{
    res.status(401)
    res.json({code:-1,msg:err.message})
})

app.listen(8001,()=>{
    console.log('port 8001 is listening!')
});
```

### 处理请求体 body-parser

> 安装 body-parser

```
npm install body-parser --save
```

```
var express = require('express');
var app = express();

const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({extended:false}))

app.use('/',mod1,mod2)

function mod1(req,res,next){
    //打印解析后的请求体
    console.log(req.body)
    console.log('进入 mod1 厂房')
    return next()
    // return next(new Error('少了一个零件，请求召回'));
}

function mod2(req,res,next){
    console.log('进入 mod2 厂房')
    res.json({msg:'数据返回结束'})
}

app.use((err,req,res,next)=>{
    res.status(401)
    res.json({code:-1,msg:err.message})
})

app.listen(8001,()=>{
    console.log('port 8001 is listening!')
});
```




