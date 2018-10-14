---
title: mongoDb入门一
date: 2018-05-10 19:33:12
tags: mongodb
---


### mongodb入门

> windows用户推荐下载3.4 （3.6会卡在一个进度不动）

[官网](https://www.mongodb.com/)

解压后一路next 然后在对应目录建立目录 

```
data/db
# 命令行中输入如下即可启动 mongodb
mongod --dbpath "f:/mongodb/data"
```

### 执行mongo命令

```
# 进入mongo命令行环境  path请自行添加
mongo 
``` 

> 如果安装不了  你可以使用官方提供的在线版(mongodb官网 doc中 /MongoDB CRUD Opeartions/Insert Document)
 


### 常用命令

```
show dbs 查看所有库
use test 使用test数据库 如果test不存在则创建
db.p   操作collections  p就是mysql表的概念  有就使用没有则创建
```

### CRUD

#### 增

> 增加的时候会默认生成 _id 确认数据的唯一标识


```
use test
db.p.insert({name:"aa"})
db.p.insert({name:"aa"},{name:"bb"})
db.p.insertOne({name:"aa"})
db.p.insertMany({name:"aa"},{name:"bb"})

#如果你想不要生成的 _id 就可以传递该自动进行覆盖
db.p.insertOne({_id:"hello",name:"aa"})
```

#### 查

```
# 查所有
db.p.find()   / db.p.find({})

# 按字段查
db.p.find({name:"aa"})

# 多个字段  name为aa age为 18
db.p.find({name:"aa",age:18})
# find返回的是数组
# findOne() 返回匹配的第一个文档
db.p.findOne({age:18}).name

# find返回的个数
db.p.find().count()
``` 

#### 修改

```
假如数据为{_id:Object("3124912ui9012i8fsdafdsf") ,name:"唐僧",age:40,gender:"man"}
db.p.update({name:"唐僧"},{age:999})
数据更新为 {_id:Object("3124912ui9012i8fsdafdsf"),age:999}
update默认 会覆盖
```

修改操作符避免覆盖 $set

```
db.p.update({_id:Object("3124912ui9012i8fsdafdsf")},
            {
                $set:{
                    name:"沙和尚"
                }
            }
)
```

删除文档的指定属性$unset

```
db.p.update({_id:Object("3124912ui9012i8fsdafdsf")},
            {
                $unset:{
                    gender:"这个值写不写没关系因为是删除的"
                }
            }
)
```

update默认只会改第一个匹配的数据

```
db.p.updateOne({name:"八戒"},
    {
        $set:{
            address:"天宫"
        }
    }
)

db.p.updateMany({name:"八戒"},
    {
        $set:{
            address:"天宫"
        }
    }
)

# 我就要用 update更新多条数据
db.p.update({name:"八戒"},
    {
        $set:{
            address:"天宫"
        }
    },
    {
        multi:true  //修改多条
    }
)

#替换一个文档
db.p.replaceOne({name:"八戒"}, { address:"天宫" })
```

#### 删除

```
db.p.remove()  不传递参数报错
db.p.remove({})  全删了，性能比较查  一个一个删
db.p.remove({name:"aa"})   //默认删除多个
db.p.remove({name:"aa"},true)  //删除一个
db.p.deleteOne({name:"aa"})
db.p.deleteMany({name:"aa"})
```

删除相关操作

```
db.p.remove({})  全删了，集合还在，性能比较差  一个一个删
db.p.drop()  清空集合，性能好 集合没了
db.dropDatabase() 删库
```
