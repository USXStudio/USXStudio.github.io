---
title: MySQL007python连接数据库
date: 2018-06-27 20:54:42
tags: mysql
---

### python连接mysql

> 注意我是python3版本 所以数据库,如果你实在不知道咋配置就参考这个(问题检索是你该具备的能力)

如果安装好了 python3,可以用pip下载mysql工具包

```
pip install PyMySQL
# 有可能会提示你pip版本低，于是你按照命令升级，然后不小心ctrl c了，那就直接去python官网重新下载一个覆盖安装即可
```

[windows下python连接数据库](https://jingyan.baidu.com/article/da1091fb1272be027849d694.html)

待更新(我想更新个mac版)

#### Connection对象
- 用于建立与数据库的连接
- 创建对象：调用connect()方法

conn=connect(参数列表)

- 参数host：连接的mysql主机，如果本机是'localhost'
- 参数port：连接的mysql主机的端口，默认是3306
- 参数db：数据库的名称
- 参数user：连接的用户名
- 参数password：连接的密码
- 参数charset：通信采用的编码方式，默认是'gb2312'，要求与数据库创建时指定的编码一致，否则中文会乱码

##### 对象的方法

- close()关闭连接
- commit()事务，所以需要提交才会生效
- rollback()事务，放弃之前的操作
- cursor()返回Cursor对象，用于执行sql语句并获得结果

#### Cursor对象

- 执行sql语句
- 创建对象：调用Connection对象的cursor()方法

```
cursor1=conn.cursor()
```

#### 对象的方法

- close()关闭
- execute(operation [, parameters ])执行语句，返回受影响的行数
- fetchone()执行查询语句时，获取查询结果集的第一个行数据，返回一个元组
- next()执行查询语句时，获取当前行的下一行
- fetchall()执行查询时，获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回
- scroll(value[,mode])将行指针移动到某个位置
        1. mode表示移动的方式
        2. mode的默认值为relative，表示基于当前行移动到value，value为正则向下移动，value为负则向上移动
        3. mode的值为absolute，表示基于第一条数据的位置，第一条数据的位置为0

#### 对象的属性

- rowcount只读属性，表示最近一次execute()执行后受影响的行数
- connection获得当前连接对象

#### CRUD

> 增

```
import pymysql

try:
    # 连接数据库
    db = pymysql.connect(host="localhost",user="root",password="123456",db="test170910",port=3306,charset="utf8")
    #创建游标对象
    cursor = db.cursor() 
    # sql语句
    sql = 'insert into students(name,gender,birthday) values("郭大侠",1,"2018-01-11")';
    #执行SQL语句
    cursor.execute(sql) 
    #提交
    db.commit()
except Exception as e:
        db.rollback() 
        print(e)
finally:
        db.close()
```

> 更新

```
import pymysql

try:
    # 连接数据库
    db = pymysql.connect(host="localhost",user="root",password="123456",db="test170910",port=3306,charset="utf8")
    cursor = db.cursor() #创建游标对象
    # sql语句
    sql = 'update  students set name="郭一一" where id >3';
    #执行SQL语句
    cursor.execute(sql) 
    #提交
    db.commit()
    print('ok')
except Exception as e:
        db.rollback() 
        print(e)
finally:
        db.close()
```

> 删

```
import pymysql

try:
    # 连接数据库
    db = pymysql.connect(host="localhost",user="root",password="123456",db="test170910",port=3306,charset="utf8")
    cursor = db.cursor() #创建游标对象
    # sql语句
    sql = 'delete from students where id =3';
    #执行SQL语句
    cursor.execute(sql) 
    #提交
    db.commit()
    print('ok')
except Exception as e:
        db.rollback() 
        print(e)
finally:
        db.close()
```

> 查

```
import pymysql

try:
    # 连接数据库
    db = pymysql.connect(host="localhost",user="root",password="123456",db="test170910",port=3306,charset="utf8")
    cursor = db.cursor() #创建游标对象
    # sql语句
    sql = 'select * from students';
    #执行SQL语句
    cursor.execute(sql) 
    #获取返回数据
    print('--------------------')
    data = cursor.fetchone() 
    print(data)
    print('--------------------')
except Exception as e:
        db.rollback() 
        print(e)
finally:
        db.close()
```

> 其他语句

- cursor对象的execute()方法，也可以用于执行create table等语句
- 建议在开发之初，就创建好数据库表结构，不要在这里执行

#### 参数化

> 保证数据库的安全性,解决sql注入

```
select * from students where name='a'

如果 'a' ===> 'a' or 1 = 1 就会导致全部被查出来 1=1为true

参数化
1. sql语句作为第一个参数  
2. @name的值作为第二个参数(进行一系列的判断，来有效阻止sql注入)
select * from students where name=@name
```

实际使用

```
sname = '对应的一个值'
# 参数是一个列表的形式
params =[sname] 
# 这里的%s不是 print()里的格式化输出  对应 params里的值
sql = 'insert into students(name) values(%s)'
count = cursor.execute(sql,params)
```

#### 查询

> 查一条记录

```
cur.execute('select * from students where id=7')
result=cur.fetchone()
print(result)
```

> 查询多行数据

```
cur.execute('select * from students')
result=cur.fetchall()
print(result)
```

#### 封装

```
      
```



