---
title: MySQL002CRUD
date: 2018-06-23 19:58:16
tags: mysql
---

### CRUD

#### 查

查所有

```
select * from 表名 
```

#### 增加

- 全列插入 insert into 表名 values(...);
- 缺省插入 insert into 表名(列1,列2) values(值1,值2);
- 同时插入多条数据 insert into 表名 values(...),(...);
 或者  insert into 表名(列1,列2) values(值1,值2),(值1,值2),(值1,值2);

```
# 全列
insert into students values(0,'八戒',1,'1990-1-1',0);
# 缺省插入
insert into students(name) values('唐僧');

#批量
insert into students values(1,'aaa',1,'1990-1-1',0),(2,'bbb',1,'1990-1-1',0),(3,'ccc',1,'1990-1-1',0);

insert into students(name) values('唐僧'),('八戒'),('悟空');
```

- 主键列是自动增长，但是在全列插入的时候要占位，通常使用0，插入成功后以实际数据为准

#### 修改

```
# 对满足条件的行修改
update 表名 set 列1=值,... where条件 

如
# 把id=2的 生日修改了
update students set birthday='1990-1-1' where id=2; 

# 不写条件就是全部改
update students set gender=0,birthday='2018-6-23' where id=6;

```

#### 删除

物理删除，数据真的没了

```
delete from 表名 where 条件
```

> 逻辑删除表里设计这样一个字段 isDelete 默认为0，当值为1时代表删除了，查询的时候只取isDelete=0的



