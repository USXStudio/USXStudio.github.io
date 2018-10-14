---
title: Schema和Model
date: 2018-05-10 23:17:24
tags: mongodb
---

> #### 我使用的mongodb是 4.11.4

### Schema

```
const mongoose = require('mongoose');
const uri = 'mongodb://localhost/test';
mongoose.connect(uri);
const db = mongoose.connection;

const Schema = mongoose.Schema;


// 创建Schema模式对象 
const UserSchema = new Schema({
    name:String,
    age:Number
})

/*
1.通过Schema创建Model
    Model代表 数据库中的集合collections
    mongodb命令行里使用要加s  如 db.users.find()
2. mongoose.model(集合名称,模型对象)
*/
const UserModel = mongoose.model('user',UserSchema);

db.on('open',()=>{console.log('db open ')}) 
db.on('error',(err,res)=>{console.log('db error')}) 
db.on('close',(err,res)=>{console.log('db close')})
```

> #### 运行如上代码数据库连接成功后  mongodb中并不会立刻创建 collection,需要向数据库中插入文档才会创建

```
UserModel.create({
        name:'hjx',
        age:18
    },function(err){
        if(!err){
            console.log("创建成功")
        }
    })
```

#### Schema模型对象的规则约束

```
const UserSchema =Schema({
    // type:类型
    // require 是否必须
    // unique 在mongodb里创建索引   类似mysql唯一主键
    // enum:['aa','bb']  指定他的值必须是什么
    name:{type:String,require:true,unique:true,enum:['hjx','lisi']},
    // 最简单写法
    // age:Number
    // 数字复杂的校验 
    // max 最大值
    // min 最小值  如果是数组   第一个值是最小范围  第二个值是报错信息
    age:{type:Number,max:90,min:[18,'不能小于18岁']}
})
```

### Model

> #### 把全局的Pormise挂载到 mongoose.Promise上

> #### 使用 async、await解决异步问题

#### 增

```
const mongoose = require('mongoose');
const uri = 'mongodb://localhost/test';
mongoose.connect(uri);
const db = mongoose.connection;
const Schema = mongoose.Schema;

mongoose.Promise = global.Promise;

// 表单对象
const UserSchema =new Schema({
    name:String,
    age:Number
})

const UserModel = mongoose.model('user',UserSchema);

(async()=>{
    let created = await UserModel.create({
        name:'hjx',
        age:18
    }).then()
    return created;
})().then(r=>{
    console.log(r)
}).catch(e=>{

})

db.on('open',()=>{console.log('db open ')}) 
db.on('error',(err,res)=>{console.log('db error')}) 
db.on('close',(err,res)=>{console.log('db close')})
```

#### 实例方法

```
const UserSchema = new Schema({
    name:String,
    age:Number
})

const UserModel = mongoose.model('user',UserSchema);
// 实例方法
UserSchema.methods.sayYourName = function(){
    return this.name;
}
// 静态方法
UserSchema.statics.findByName = async function(name){
    return await this.findOne({name:name})
}

// 具体的表   命令行里使用要加s  如   db.users.find()
const UserModel = mongoose.model('user',UserSchema);

// 实例方法
(async()=>{
    let found = await UserModel.findOne({});
    console.log(found.sayYourName())
})().then(r=>{
    console.log(2)
}).catch(e=>{

})

// 静态方法
(async()=>{
    let found = await UserModel.findByName('hjx');
    return found;
})().then(r=>{
    console.log(r)
}).catch(e=>{

})

//保存
(async()=>{
   let user = new UserModel({name:'lisi',age:19})
   return await user.save()
})().then(r=>{
}).catch(e=>{
    console.log(e)
})
```

工厂模式

```
// 工厂模式
(async(params)=>{
   let flow = UserModel.find({});
   flow.where('age').lt(21);
   flow.select({name:1})
   flow.skip(0);
   if(params.sort) flow.sort(params.sort);
   let res = await flow.then()
   return res;
})({
    sort:'-age'
})
.then(r=>{
    console.log(r)
})
.catch(e=>{
    console.log(e)
})
```

