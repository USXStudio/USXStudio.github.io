---
title: mongoDb练习三
date: 2018-05-10 22:16:24
tags: mongodb
---

### 练习3

1. 查询工资小于1000 或 大于3000的数据

```
db.emp.find({
    $or:[
        sal:{
            $lt:1000
        },
        sal:{
            $gt:3000
        }
    ]
})
```

2. 为所有工资低于1000的 员工 涨薪 400

```
db.emp.updateMany(
    {sal:{$let:1000}},
    {$inc:{sal:400}}
)
```

#### 排序 sort()

查询文档时，默认按_id的值进行升序排列

- sort()需要传递一个对象指定对应列进行排序
- 1代表升序
- -1代表降序
- 多个排序  sort({sal:1,empno:-1}) 先按照工资排序 然后再按照 部门排序
- limit skip sort 可以任意顺序调用


```
按工资升序排列
db.emp.find({}).sort({sal:1})
按工资降序排列
db.emp.find({}).sort({sal:-1})
```

#### 设置投影 (如果我只想看员工姓名/工资，不想看_id)

```
db.emp.find({},{name:1,sal:1,_id:0})
```


