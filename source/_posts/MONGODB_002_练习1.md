---
title: mongoDb练习一
date: 2018-05-10 21:01:23
tags: mongodb
---

### 练习1

1. 进入my_test数据库

```
use my_test
```

2. 向 my_test中 user集合插入一个文档

集合习惯会在其后加s  所以 user在操作时是 db.users

```
db.users.insert({username:"猪八戒"})
```

3. 查询user集合中的文档

```
db.users.find()
```

4. 向user集合中插入一个文档

```
db.users.insert({username:"孙悟空"})
```

5. 统计user集合中文档document的数量

```
db.users.find().count()
```

6. 查询user集合中 username为孙悟空的值

```
db.users.find({username:"孙悟空"})
```

7. 向user集合中 username="孙悟空"的文档添加一个 address= 花果山

```
db.users.update({username:"孙悟空"},{
    {
        $set:{
            {address:"花果山"}
        }
    }
})
```

8. 使用{username:"唐僧"}替换  username为 猪八戒 的文档

```
db.users.replaceOne({username:"猪八戒"},{username:"唐僧"})
```

9. 删除 username 为 孙悟空 的 address属性

```
db.users.update({username:"孙悟空"},{$unset:{address:""}})
```

10. 向 username 为 孙悟空 的文档中 添加一个 hobby:{cities:["北京","上海","深圳"],movies:["三国","英雄"]}

```
db.users.update({username:"孙悟空"},{
    $set:{
        hobby:{
            cities:["北京","上海","深圳"],
            movies:["三国","英雄"]
        }
    }
})
```

> mongodb 中的属性值也可以是一个文档，当一个文档的一个属性值是文档时，我们称之为「内嵌文档」

11. 向 username 为 唐僧 的文档中 添加一个 hobby:{movies:["大话西游","天下第一"]}

```
db.users.update({username:"孙悟空"},{
    $set:{
        hobby:{
           movies:["大话西游","天下第一"]
        }
    }
})
```

12. 查询喜欢 电影 movies = 英雄 的文档

- mongodb支持直接通过内嵌文档的属性进行查询，使用「.」进行操作 形如 hobby.movies的形式
- 通过内嵌文档查询 必须使用引号包起来 形如 "hobby.movies"

```
db.users.find({"hobby.movies":"英雄"})
```

13. 向 唐僧 的文档中,电影 movies 中添加 "寒战"

- 注意此时不要用 $set 修改操作符
- 使用 $push 用于向数组中添加一个新的元素(可重复添加)
- $addToSet 向数组中添加一个元素 (如果添加元素已经存在就不会再去添加了，不可重复添加)

```
db.users.update({username:"唐僧"},{
    $push:{
        "hobby.movies":"寒战"
    }
})

db.users.update({username:"唐僧"},{
    $addToSet:{
        "hobby.movies":"寒战"
    }
})
```

14. 删除 喜欢 北京 的用户

```
db.users.remove({"hobby.cities":"北京"})
```

15. 删除 users集合

```
db.users.remove({}) 
db.users.drop()
```

