---
title: MySQL005关系
date: 2018-06-24 15:28:16
tags: mysql
---

### 关联

> 查询男生的姓名，总分

为啥不以姓名分组，因为有同名的情况

```
select name,sum(score)
from students 
inner join scores on students.id=scores.stuid
where gender=1
group by students.id;
```

> 查询科目的名称，平均分

```
select subjects.title,avg(scores.score)
from scores 
inner join subjects on scores.subid=subjects.id
group by subjects.title;
```

> 查询未删除科目的名称，最高分、平均分

```
select subjects.title,avg(scores.score),max(scores.score)
from scores 
inner join subjects on scores.subid=subjects.id
where subjects.isdelete=0
group by subjects.title;
```

### 自关联

省市区

省 | 市 | 区
- | :-: | -: 
id|id|id
name|name|name
-|pid省的引用|pid市的引用

我们知道中国36个省，但是这样就创建了一个表显然很浪费

我们假设

- 省 36
- 市 1万
- 区 10万

> 没有必要建立三张表去存这些数据,如果我们能建立一张表把这些数据存起来

每建一张表对数据库的开销是很大的

1. 我们能不能给省加一个pid
2. 给省的pid为null
3. 合成了一个表外键还是需要的引谁呢？ 市引用省，区引用市

id | name | pid
- | :-: | -: 
1|河南省|null
2|郑州市|1

> 自引用,自关联

- 不用增加新表的开销

```
create table areas(
    aid int primary key auto_increment not null,
    name varchar(20),
    pid int,
    foreign key(pid) references areas(id)
);
```

> 自关联查询山西省的市

```
select sheng.aid as sid,sheng.name as sname,
shi.aid as shiid,shi.name as shiname
from areas as sheng
inner join areas as shi on sheng.aid=shi.aid
where sheng.pid is null and sheng.name='山西省'
;

```