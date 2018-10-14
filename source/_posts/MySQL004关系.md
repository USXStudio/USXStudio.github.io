---
title: MySQL004关系
date: 2018-06-24 11:28:16
tags: mysql
---

### 关系

- 实体与实体之间有3种对应关系，这些关系也需要存储下来
- 在开发中需要对存储的数据进行一些处理，用到内置的一些函数
- 视图用于完成查询语句的封装
- 事务可以保证复杂的增删改操作有

> 请看如下情况

学生表 | 成绩表 | 科目表
- | :-: | -: 
id|id|id
name|score|title
-|学生的引用|-
-|科目的引用|-

学生列的数据不是在scores表里建立的,而应该从学生表引用过来,关系也是一条数据;根据范式,应该存储学生的唯一编号(id),而不是学生的姓名或其他信息,同理科目表也是关系列

> 创建表如下

```
create table scores (
    id int primary key auto_increment,
    stuid int,
    subid int,
    score decimal(5,2)
);
```

#### 外键约束

班级表 | 学生表 
- | :-: 
id|id
name|name
-|班级的引用

假如有班级表只有三个班

```
学生添加的时候如果添加班级4如何去验证班级是否存在？

此时就会有外键约束
```

- 必须是班级表中存在的数据,可以通过外键约束进行数据的有效性验证
- 为classid添加外键约束

> 为刚刚的分数表添加外键约束

此时插入或修改数据时,如果stuid的值在students表里不存在就会报错

```
create table scores (
    id int primary key auto_increment,
    stuid int,
    subid int,
    score decimal(5,2),
    foreign key(stuid) references students(id),
    foreign key(subid) references subjects(id),
);
```

> 已经建表了再进行添加约束

```
alter table scores add constraint stu_score foreign key(stuid) references students(id);


stu_score 代表约束名字(语法要求)
```

#### 外键的级联操作

- 在删除students表的数据时，如果这个id值在scores中已经存在，则会抛异常
- 推荐使用逻辑删除，还可以解决这个问题
- 可以创建表时指定级联操作，也可以在创建表后再修改外键的级联操作

> 语法
```
alter table scores add constraint stu_sco foreign key(stuid) references students(id) on delete cascade;
```

> 级联操作的类型包括：

- restrict（限制）：默认值，抛异常
- cascade（级联）：如果主表的记录删掉，则从表中相关联的记录都将被删除
- set null：将外键设置为空
- no action：什么都不做

### 连接查询

分数表查询后是如下结果

```
select * from scores;
```

 id| score | stuid | subid
- | :-: | :-: | - 
0|100|1|1
1|99|1|2
2|78|2|1
3|60|2|2
4|30|3|1
5|78|3|2

但是这样根本看不出谁是谁啊

```
select students.name,subjects.title,scores.score    
from scores
inner join students on scores.stuid=students.id
inner join subjects on scores.subid=subjects.id
;
```

- inner join 后的表先写谁后写谁没关系
- 但是inner join subjects on之后的 scores.subid必须和subjects有关系


#### 连接查询的分类

- 内连接

连接表的数据都能对应才会出现(都匹配才出现，即如有学生黄药师但是他没有成绩，就不会查出来) 

```
select * 
from scores
inner join  students on students.id=scores.stuid
;
```

 id| score | stuid | subid |name|gender|birthday|isDelete
- | :-: | :-: | :-: | :-: | :-:| :-: | -
0|100|1|1|黄蓉|0|1990-02-11 00:00:00|0
1|99|1|2|黄蓉|0|1990-02-11 00:00:00|0
2|100|2|1|郭靖|1|1990-02-11 00:00:00|0
3|99|2|2|郭靖|1|1990-02-11 00:00:00|0
4|78|3|1|杨过|1|1990-02-11 00:00:00|0
5|78|3|2|杨过|1|1990-02-11 00:00:00|0
6|78|4|1|小龙女|0|1990-02-11 00:00:00|0
7|78|4|2|小龙女|0|1990-02-11 00:00:00|0


- 左连接代表以 from后的 表名为准的数据全查出来 连接的表数据没有就用null填充

```
select * 
from students
left join scores on students.id=scores.stuid
;
```

 id| name|gender|birthday|isDelete|id1|score | stuid | subid |
- | :-: | :-: | :-: | :-: | :-:| :-: |:-:| -
1|黄蓉|0|1990-02-11 00:00:00|0|0|100|1|1|
1|黄蓉|0|1990-02-11 00:00:00|0|1|99|1|2|
2|郭靖|1|1990-02-11 00:00:00|0|2|100|2|1
2|郭靖|1|1990-02-11 00:00:00|0|3|99|2|2|
3|杨过|1|1990-02-11 00:00:00|0|4|78|3|1|
3|杨过|1|1990-02-11 00:00:00|0|5|78|3|2|
4|小龙女|0|1990-02-11 00:00:00|0|6|78|4|1
4|小龙女|0|1990-02-11 00:00:00|0|7|78|4|2|
5|黄药师|1|1990-02-11 00:00:00|0|null|null|null|null|null
6|郭襄|0|1990-02-11 00:00:00|0|null|null|null|null|null


- 右连接 跟左连接类似

```
select * 
from scores 
left join students on students.id=scores.stuid
;
```

 id1|score | stuid | subid |id| name|gender|birthday|isDelete|
- | :-: | :-: | :-: | :-: | :-:| :-: |:-:| -
0|100|1|1|1|黄蓉|0|1990-02-11 00:00:00|0|
1|99|1|2|1|黄蓉|0|1990-02-11 00:00:00|0|
2|100|2|1|2|郭靖|1|1990-02-11 00:00:00|0|
3|99|2|2|2|郭靖|1|1990-02-11 00:00:00|0|
4|78|3|1|3|杨过|1|1990-02-11 00:00:00|0|
5|78|3|2|3|杨过|1|1990-02-11 00:00:00|0|
6|78|4|1|4|小龙女|0|1990-02-11 00:00:00|0|
7|78|4|2|4|小龙女|0|1990-02-11 00:00:00|0|
null|null|null|null|5|黄药师|1|1990-02-11 00:00:00|0|
null|null|null|null|6|郭襄|0|1990-02-11 00:00:00|0|


#### 查询平均分

```
select name,ave(scores) as avg1 from scores
inner join students on scores.stuid=students.id
group by stuid
order by avg1
```


