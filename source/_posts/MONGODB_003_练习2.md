---
title: mongoDb练习二
date: 2018-05-10 21:36:47
tags: mongodb
---

### 练习2

1. 向 numbers 集合中插入20000条数据

笨方法

```
db.numbers.insert({num:1})
db.numbers.insert({num:2})
db.numbers.insert({num:3})
db.numbers.insert({num:4})
...
db.numbers.insert({num:20000})
```

稍微快一点

```
for(var i=1;i<=20000,i++){
    db.numbers.insert({num:i})
}
```

快一点

```
var arr =[];
for(var i=1;i<=20000,i++){
    arr.push({num:i})
}

db.numbers.insert(arr)
```

2. 查询numbers 中 num 为500的文档

```
db.numbers.find({num:500})
```

3. 查询 num 大于500的文档

> $lt 小于  $gt大于  $eq等于  $ne 不等于

```
db.numbers.find({num:{$gt:500}})
```

4. 查询 num 大于等于500的文档

```
db.numbers.find({num:{$gte:500}})
```

5. 查询num 大于40 小于50的文档

```
db.numbers.find({num:{$gt:40,$lt:50}})
```

6. 查看 numbers集合中 前10条数据

> #### limit()设置数据的上限

```
不靠谱的方式
db.numbers.find({num:{$lte:10}})

靠谱的方式
设置显示数据的上限
db.numbers.find().limit(10)
```

> ### 开发中绝对不该执行不带条件的查询

```
db.numbers.find()
```

7. 查询numbers  11条到20条的数据

> 分页skip()

```
1-10
11-20
21-30 
...
skip()用于跳过指定的条数
```

```
db.numbers.find().skip(10).limit(10)
```

8. 查询numbers  21条到30条的数据

```
db.numbers.find().skip(20).limit(10)
```

### 分页公式

```
skip((pageNo-1)*pageSize).limit(pageSize)
```

### 注意事项

```
mongodb会自动调整 skip和limit的位置
skip() 和 limit()的顺序谁先谁后无所谓
```