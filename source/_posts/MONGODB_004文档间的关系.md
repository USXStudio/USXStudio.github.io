---
title: mongoDb文档之间的关系
date: 2018-05-10 21:54:39
tags: mongodb
---

### 文档之间的关系

#### 一对一 one to one

- 夫妻
- Mongodb中 通过内嵌文档体现 一对一的关系

```
db.test.insert([
    {
        name:"黄蓉",
        husband:{
            name:"郭靖"
        }
    },
    {
        name:"小龙女",
        husband:{
            name:"杨过"
        }
    }
])
```


#### 一对多 one to many  / many to one

- 父母  孩子
- 用户  订单
- 文章  评论

一对多的关系 ==>  用户&&订单

```
db.users.insert([
    {username:"aaa"},
    {username:"bbb"}
])

/*
{_id:ObjectId("5aaaaaaaaaaaaaa"),username:"aaa"},
{_id:ObjectId("5bbbbbbbbbbbbbb"),username:"bbb"}
*/

订单
db.order.insert({
    list:["苹果","香蕉","葡萄"]
    user_id:ObjectId("5aaaaaaaaaaaaaa")
})

db.order.insert({
    list:["华为","平板","iPad"]
    user_id:ObjectId("5bbbbbbbbbbbbbb")
})

查询用户名 aaa的订单
var aaaId = db.users.find({username:"aaa"})
db.order.find({user_id:aaaId})
```



#### 多对多 many to many

- 分类  商品
- 老师  学生

```
db.teachers.insert([
    {name:"欧阳锋"},
    {name:"黄药师"},
    {name:"洪七公"}
])

db.students:insert([
    {
        name:"郭靖",
        tech_ids:[
            ObjectId("5aaaaaaaaaaa"),
            ObjectId("5bbbbbbbbbbb"),
            ObjectId("5ccccccccccc"),
        ]
    },
    {
        name:"孙悟空",
        tech_ids:[
            ObjectId("5aaaaaaaaaaa"),
            ObjectId("5bbbbbbbbbbb")
        ]
    }
])
```

#### 使用最广泛的关系是 一对多