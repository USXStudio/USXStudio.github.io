---
title: MySQL003查询强化
date: 2018-06-23 22:58:16
tags: mysql
---

### 查询

> 占据90%的业务

```
select * from 表名 ;
```

- from关键字后面写表名，表示数据来源于是这张表
- select后面写表中的列名，如果是*表示在结果中显示表中所有列
- 在select后面的列名部分，可以使用as为列起别名，这个别名出现在结果集中
- 如果要查询多个列，之间使用逗号分隔

> 查询所有

```
select * from students;
##########################################################
| id | name | birthday              | gender | isDelete |
##########################################################
  1    郭靖   2017-01-01 00:00:00       1
  2    黄蓉   2017-01-01 00:00:00       0
  3    郭芙   2017-01-01 00:00:00       0
  4    杨过   2017-01-01 00:00:00       1
```

> 查询 id和name列

```
select id,name from students;
##########################################################
  id   name
  1    郭靖  
  2    黄蓉   
  3    郭芙   
  4    杨过 
```

> 查询gender

```
select gender from students;
##########################################################
gender
1
0
0
1
```
> 想看gender有多少种(去重复)

- distinct消除重复的行
- 如果是多列比较的整个一行的每列进行去重复(如果你查了id肯定没法去重)

```
select distinct gender from students;
##########################################################
gender
1
0
```

#### 条件

```
select * from 表名 where 条件;
```

> where之后的内容为条件，它是对行进行筛选

##### 比较运算符

```
select * from students where id=3;
select * from students where id>3;
select * from students where id>=3;
select * from students where id<3;
select * from students where id<=3;
```

> 查询名字为「黄蓉」和的学生

```
select * from students where name='黄蓉';
```

> 查询名字不为「黄蓉」和的学生

```
select * from students where name!='黄蓉';
```

##### 逻辑运算符

- and
- or
- not

> 学号大于3的女同学

```
select * from students where id>3 and gender=0;
```

> 编号小于4或没被删除的学生

```
select * from students where id<4 or isdelete=0;
```

##### 模糊查询

- like
- %任意个字符
- _一个任意字符

> 查询姓黄的学生

```
select * from students where name like '黄%';
```

> 查询姓名为黄且总共两个字的学生

```
select * from students where name like '黄_';
```

> 查询姓名为黄且总共三个字的学生

```
select * from students where name like '黄__';
```

> 查询姓名为黄或者叫靖的学生

```
select * from students where name like '黄%' or name like '%靖';
```

##### 范围查询

- 连续的范围
- 不连续的范围

> in表示不连续的范围

查询编号是1,3,8的学生

```
select * from students where id in(1,3,8);
```

> between ... and ...表示连续的范围

查询编号是3-8的学生

```
select * from students where id between 3 and 8;
```

查询编号是3-8的「男」学生

```
select * from students where id between 3 and 8 and gender=1;
```

##### 空判断

- null与''是不同的
- 判空is null

> 查询没写地址的学生

```
select * from students where address is null;
```

> 查询写了地址的学生

```
select * from students where address is not null;
```

> 查询写了地址的女学生

```
select * from students where address is not null and gender=0;
```

##### 优先级

- 小括号,not,比较运算符,逻辑运算符
- and比or先运算，如果同时出现并希望先算or,需要使用()

### 聚合

将现有的多行数据进行统计，统计的结果是你看不到任何原始数据了，只看到结果

- count
- max
- min
- sum
- avg

> count(*)表示计算总行数，括号中写星与列名，结果是相同的

```
查询学生总数
select count(*) from students;

查询学生总数未被删除的
select count(*) from students where isdelete=0;
```

> max(列)表示求此列的最大值
```
查询女生的编号最大值
select max(id) from students where gender=0;
```
> min(列)表示求此列的最小值
```
查询未删除的学生最小编号
select min(id) from students where isdelete=0;
```
> sum(列)表示求此列的和
```
查询男生的编号之后
select sum(id) from students where gender=1;
```
> avg(列)表示求此列的平均值
```
查询未删除女生的编号平均值
select avg(id) from students where isdelete=0 and gender=0;
```

```
查询最小编号的学生信息  ————子查询
select * from students where id =(
    select min(id) from students where isdelete=0
);
```

### 分组

- 按照字段分组，表示此字段相同的数据会被放到一个组中
- 分组后，只能查询出相同的数据列，对于有差异的数据列无法出现在结果集中
- 可以对分组后的数据进行统计，做聚合运算

语法：
```
select 列1,列2,聚合... from 表名 group by 列1,列2,列3...
```

> 查询男女生总数
```
select gender as 性别,count(*)
from students
group by gender;
```

> 查询各城市人数
```
select hometown as 家乡,count(*)
from students
group by hometown;
```

#### 分组后筛选

语法：

```
select 列1,列2,聚合... from 表名
group by 列1,列2,列3...
having 列1,...聚合...
```

- having后面的条件运算符与where的相同

> 查询男生总人数

```
方案一
select count(*)
from students
where gender=1;
-----------------------------------
方案二：
select gender as 性别,count(*)
from students
group by gender
having gender=1;
```

#### 对比where与having
- where是对from后面指定的表进行数据筛选，属于对原始数据的筛选
- having是对group by的结果进行筛选

```
原始集合——>where——>结果集——>分组>结果集——>having——>
```

### 排序

>为了方便查看数据，可以对数据进行排序

语法：

```
select * from 表名
order by 列1 asc|desc,列2 asc|desc,...
```

- 将行数据按照列1进行排序，如果某些行列1的值相同时，则按照列2排序，以此类推
- 默认按照列值从小到大排列
- asc从小到大排列，即升序
- desc从大到小排序，即降序

> 查询未删除男生学生信息，按学号降序
```
select * from students
where gender=1 and isdelete=0
order by id desc;
```

> 查询未删除科目信息，按名称升序
```
select * from subject
where isdelete=0
order by stitle;
```

### 分页

获取部分行

> 当数据量过大时，在一页中查看数据是一件非常麻烦的事情

语法
```
select * from 表名 limit start,count;
```

- 从start开始，获取count条数据
- start索引从0开始

##### 示例：分页

> 已知：每页显示m条数据，当前显示第n页

>求总页数：此段逻辑后面会在python中实现

- 查询总条数p1
- 使用p1除以m得到p2
- 如果整除则p2为总数页
- 如果不整除则p2+1为总页数

求第n页的数据
```
select * from students
where isdelete=0
limit (n-1)*m,m
```

### 总结

> 完整的select语句

```
select * from 表名
where ...
group by ... having ...
order by ...
limit start,count
```

> 执行顺序

- from 表名
- where 条件
- group by 列名
- select distinct *
- having ...
- order by ...
- limit start,count