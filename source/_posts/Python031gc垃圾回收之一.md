---
title: Python031gc垃圾回收
date: 2018-06-23 12:10:16
tags: python
---

### 垃圾回收

避免垃圾产生的方式

1. 小整数对象池

> python 对小整数的定义是[-5,257)这些整数对象是提前建好的,不会被垃圾回收

> 单个字符同理

```
a = 100
b = 100
c = 100

id(a) # 532323123
id(b) # 532323123
id(c) # 532323123
都是相同的地址
```
2. 大整数对象池

每个大整数,均创建一个新的对象

```
a = 1000
b = 1000
c = 1000

id(a) # 4532323123
id(b) # 1235567788
id(c) # 5215567888
```

3. intern机制

```
a1 = "helloworld"
a2 = "helloworld"
a3 = "helloworld"
a4 = "helloworld"
a5 = "helloworld"
a6 = "helloworld"
a7 = "helloworld"
a8 = "helloworld"
a9 = "helloworld"
不会创建9 个对象 会共用一份

例外就是 字符串里不能包含特殊字符 如 空格
b1 = "hello world"
b2 = "hello world"
b3 = "hello world"
b4 = "hello world"
b5 = "hello world"
b6 = "hello world"
b7 = "hello world"
b8 = "hello world"
b9 = "hello world"

这样就会创建9个对象
```

#### 总结

- 小整数[-5,257)共用对象，常驻内存
- 单个字符共用对象，常驻内存
- 单个单词，不能修改,默认开启intern机制,共用对象,引用计数为0则销毁

### 什么是gc(garbage collection)垃圾回收

python采用引用计数为主，标记清除为辅的策略

#### 引用计数

> 优点

- 简单
- 时效性:一旦没有被引用就释放内存

> 缺点

- 维护起来耗费资源
- 循环引用

```
list1 = []
list2 = []
list1.append(list2)
list2.append(list1)
俩个列表互相引用。占用的内存无法回收
```

#### 标记清除（待更新）

> 高深的知识点

