---
title: MySQL001基本概念
date: 2018-06-23 19:08:16
tags: mysql
---

### 数据库

#### 数据库三范式

1. 列不可拆分
2. 唯一标识
3. 引用主键

每一个范式都是在前一个范式基础上建立的

#### 字段类型

类型有很多，这里列出常用的

- 数字 int, decimal
- 字符串 char,varchar,text
- 日期 datetime
- 布尔 bit

#### 约束

- 主键primary key
- 非空 not null
- 唯一 unique
- 默认 default
- 外键 foreign key 

> 创建数据库

```
create database 数据库名 charset=utf-8;
```

> 删除数据库

```
drop database 数据库名;
```

> 切换数据库

```
use 数据库名;
```

> 查看当前数据库

```
select database();
```

### 表操作

> 查看表

```
show tables;
```

- 创建表
- auto_increment表示自增长

```
create table 表名(列及类型);
如
create table students2 (
    id int auto_increment primary key,
    sname varchar(10) not null
);
```

> 查看表设计

```
desc 表名;
```

> 修改表 alter

- 修改列的时候列名是不能改的
- 如果已经有数据了在进行增加列，容易出错

```
alter table 表名 add|change|drop 列名 类型;

alter table students add birthday datetime;

###############################逻辑删除，数据还在只是 isDelete状态变成1
alter table students add isDelete bit default 0;
```

> 删除表

```
drop table 表名;
```

> 更改表名

```
rename table 原名 to 现名
```

> 查看建表语句

```
show create table 表名
```