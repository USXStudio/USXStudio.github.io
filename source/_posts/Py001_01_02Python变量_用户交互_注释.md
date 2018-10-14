---
title: Py001_01_02Python变量_用户交互_注释
date: 2018-08-13 19:51:54
tags: M01
---

### hello world

```
print('hello world')
```

### 变量

存储数据的中间结果,以备程序后续调用。

#### 定义规范

- 字母数字下划线的任意组合
- 第一个字符不能是数字
- 不能使用python的关键字做变量名(print,pass,or,not,and...)

#### 命名习惯

> 驼峰

```
CattleId
FarmId
```

> 下划线

```
cattle_id
farm_id
```

> 特别low的方式

- 变量名为中文、拼音    张三的年龄 = 22
- 变量名过长 the_name_of_oldboy_girlfriend = 'Lisa'
- 变量名词不达意  name = 'hjx'

#### 常量

在你的程序运行中不变的量就是常量

> 很可惜在python中没有常量的定义。所以程序员约定俗成的把全部大写声明的变量定义为常量

```
CATTLE_AGE = 10
```

### 与用户交互

- input

```
name = input("请输入你的名字")
print("hello" , name)
# 也可以这样，效果一样
print("hello" + name)
```

### 注释

> python里注释使用 "#"

```
# 本行是注释
print("hello")
```
