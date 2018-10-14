---
title: express三连(一)
date: 2018-04-27 21:43:42
tags: express
---

### Router用法

simpleRouter.js

```
const router = require('express').Router();

router.get('/',(req,res)=>{
    res.json({code:0,msg:'welcome to simple root router'})
})

router.get('/simple_demo',(req,res,next)=>{
    res.end(`your simple_demo  here`)
})

module.exports = router;
```

anotherRouter.js

```
const router = require('express').Router();


router.route('/')
    .get((req,res)=>{
        res.json({code:0,msg:'welcome to another root router  get method'})
    })
    .post((req,res)=>{
        res.json({code:0,msg:'welcome to another root router post method'})
    })


router.get('/another_demo',(req,res,next)=>{
    res.end(`your another_demo  here`)
})

module.exports = router;
```

server001.js

```
var express = require('express');
var app = express();
const bodyParser = require('body-parser');

const simpleRouter = require('./simpleRouter')
const anotherRouter = require('./anotherRouter')

app.use(bodyParser.urlencoded({extended:false}))

app.use('/simple/',simpleRouter);
app.use('/another/',anotherRouter);

app.use((err,req,res,next)=>{
    res.status(401)
    res.json({code:-1,msg:err.message})
})

app.listen(8001,()=>{
    console.log('port 8001 is listening!')
});
```

### 使用express生成器(类似vue-cli脚手架的工具)

安装express模板生成器

```
npm install express-generator -g
```

创建目录(你的项目目录)

```
mkdir one_punch
```

生成模板

```
# --view==ejs 代表设置支持ejs语法的模板文件
express --view=ejs one_punch
```

切换项目目录，安装依赖

```
cd one_punch
npm install
```

启动项目

```
npm start
```

> 默认监听3000端口

打开浏览器你会看到生成的express页面

- localhost:3000 
- localhost:3000/users


