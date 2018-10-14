---
title: mongoose是什么鬼
date: 2018-05-10 22:38:55
tags: mongodb
---

### 什么是mongoose

> 在之前的文章中我们都是命令行操作mongodb

- 而实际使用中肯定不能让客户操作命令行录入数据
- mongoose是一个可以让我们通过node来操作 mongoDB的模块
- mongoose是一个ODM库（对象文档模型）

[官网](http://mongoosejs.com/)

> moogoose实际作用是让我们以「对象的形式」操作数据库

好处

1. 可以为文档创建一个模式结构（Schema）
2. 可以对模型中的对象进行验证
3. 数据可以通过类型转换为对象模型
4. 可以使用中间件来与业务逻辑挂钩
5. 比Node原生的mongodb驱动更容易

### mongoose提供的对象

- Schema （模式对象） 对字段进行约束
- Model 作为集合中所有文档的表示，相当于MongoDB中的collections
- Document 表示集合中的具体文档，相当于MongoDB中的document

### 使用mongoose

1. 下载安装 mongoose 

```
# 创建项目目录
mkdir demo
# 切换目录
cd demo
# 初始化package.json
npm init 一路回车
# 安装依赖
npm i mongoose --save
# 新建test.js
touch test.js
```

2. 引入mongoose


```
const mongoose = require('mongoose')
```

3. 连接数据库

> 如果端口号是27017可以省略

```
//引入mongoose模块
const mongoose = require('mongoose')
mongoose.connect("mongodb://数据库的ip地址:端口号/数据库名")
```

4. 监听数据库的连接状态

```
mongoose.disconnect() //一般不使用
```

全部代码

```
const mongoose = require('mongoose');
const db = mongoose.connect('mongodb://localhost/test');
db.on('open',()=>{console.log("数据库连接成功")})

db.on('close',()=>{console.log("数据库连接断开")})
```

#### 和mysql区别

- mongoDB一般只连接一次，不会主动断开（没有事务控制）除非服务器宕机
- mysql是关系型数据库，每操作数据就要进行事务控制，以至于 连接一次关闭一次 连接一次关闭一次

> 所以一般不会调用 mongoose.disconnect() 


