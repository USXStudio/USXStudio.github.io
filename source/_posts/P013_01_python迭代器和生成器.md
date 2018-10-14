---
title: P013_01_python迭代器
date: 2018-07-24 19:42:30
tags: fullstack
---

### 迭代器初识

```
a = [1,2,3]
# 索引
# 循环for

for i in a:
    print(i)

for k in dic:
    pass
```

- list
- dic
- str
- set
- tuple
- f = open()文件
- range()
- enumerate

### 迭代器的概念

> 先来一个小知识点，查看一个对象可用的方法 dir

dir(对象)

```
print(dir([])) #告诉我列表拥有的方法
print(dir({})) #告诉我字典拥有的方法
print(dir('')) #告诉我字符串拥有的方法
print(dir(range(10))) #告诉我range(10)拥有的方法
```

> 我们知道数组相加如下

```
[1] + [2] # [1,2]
实际上就是调用  [1].__add__([2])
```

> 找这些能被for循环使用的方法的交集

```
# 取这些可被for循环的数据类型的交集
ret = set(dir([]))&set(dir({}))&set(dir(''))&set(dir(range(10)))
print(ret)  #iterable  
```

只要能被for循环的数据类型，就一定有__iter__方法



```
print('__iter__' in dir(int))
print('__iter__' in dir(bool))
print('__iter__' in dir(list))
print('__iter__' in dir(dict))
print('__iter__' in dir(set))
print('__iter__' in dir(tuple))
print('__iter__' in dir(enumerate([])))
print('__iter__' in dir(range(1)))

结果：
False
False
True
True
True
True
True
True
```

#### 一个列表执行了__iter__()之后的返回值就是一个迭代器


```
print(dir([]))
print(dir([].__iter__()))
print(set(dir([].__iter__())) - set(dir([])))
print([1,'a','bbb'].__iter__().__length_hint__())  #元素个数
```

再看双下划线 next方法

```
l = [1,2,3]
iterator = l.__iter__()
print(iterator.__next__()) # 1
print(iterator.__next__()) # 2
print(iterator.__next__()) # 3
print(iterator.__next__()) # 报错

[].__iter__() 迭代器  -- > __next__  #通过next就可以从迭代器中一个一个的取值
```

> 迭代器要点：

``` 
Iterable  可迭代的    -- > __iter__  #只要含有__iter__方法的都是可迭代的
 
只要含有__iter__方法的都是可迭代的 —— 可迭代协议
```

```
l = [1,2,3,4]
for i in l.__iter__():
    print(i)
```

 
#### 迭代器的概念

> 迭代器协议 —— 内部含有__next__和__iter__方法的就是迭代器
 
#### 迭代器协议和可迭代协议

```
可以被for循环的都是可迭代的
可迭代的内部都有__iter__方法
只要是迭代器 一定可迭代
可迭代的.__iter__()方法就可以得到一个迭代器
迭代器中的__next__()方法可以一个一个的获取值
```

```
for循环其实就是在使用迭代器
iterator
可迭代对象
直接给你内存地址
print([].__iter__())
print(range(10))
```

#### for循环的本质

> 只有 是可迭代对象的时候 才能用for
> 当我们遇到一个新的变量，不确定能不能for循环的时候，就判断它是否可迭代

```
for i in l:
    pass

# for循环的本质就是迭代器
#iterator = l.__iter__()
#iterator.__next__() 
```

#### 迭代器的好处：


- 从容器类型中一个一个的取值，会把所有的值都取到。
- 节省内存空间
    迭代器并不会在内存中再占用一大块内存，
    而是随着循环 每次生成一个
    每次next每次给我一个

```
range(10000000000000000)  # 并不是立即生成，否则很浪费内存空间

list(range(10000000000000000)) # 估计内存炸了，把迭代器转为列表
```

### 生成器

本质是个迭代器

#### 生成器函数

> 1.我们先实现一个普通方法

```
def generator():
    print(1)
    return 'a'

xxx = generator()
print(xxx)

结果
1
a
```

> 2.yield关键字

将刚刚的return 换成 yield

```
def generator():
    print(1)
    yield 'a'

xxx = generator()
print(xxx)

结果：
<generator object generator at 0x000000000238C518>

------------------------------------------------------------
print(xxx.__next__())
结果：
1
a
```

####　什么是生成器

- 只要含有yield关键字的函数都是生成器函数
- yield不能和return共用而且需要写在函数内部
- 函数内部有yield关键字，这个函数执行返回的结果就是生成器，并且不会执行函数内部代码

> #### 生成器函数 ： 执行之后会得到一个生成器作为返回值

```
def xxx():
    print('====>a')
    yield 1
    print('====>b')
    yield 2
    print('====>c')
    yield 3
    print('====>end')

g=xxx()

print(g.__next__()) # 返回第一次yield关键字后面的值
print(g.__next__()) # 返回第二次yield关键字后面的值
print(g.__next__()) # 返回第三次yield关键字后面的值
print(g.__next__()) # 报错 

====>a
1
====>b
2
====>c
3
====>end
Traceback (most recent call last):
  File "D:/python_code/py20180718/001.py", line 18, in <module>
    print(g.__next__())
StopIteration
```

> 需求打印 200,0000次 哇哈哈1==》哇哈哈2000000，而我现在只要50个

```
#娃哈哈%i
def wahaha():
    for i in range(2000000):
        yield '娃哈哈%s'%i

g = wahaha()
count = 0
for i in g:
    count +=1
    if count > 50:
        break
    print('```````````'+i+'`````````````')
```


#### 生成器真的省内存吗？

> 碰巧前面我们刚学过如何测试效率和装饰器，正好拿来练习！！！

```
import time

def test(func):
    def inner(*args,**kwargs):
        start_time = time.time()
        res = func(*args,**kwargs)
        end_time = time.time()
        print("测试效率:", end_time - start_time)
        return res
    return inner

@test
def getList(n):
    l1 = list(range(n))
@test
def getGenerator(n):
    for i in range(n):
        yield i


getList(100000)
getGenerator(100000)


结果：
测试效率: 0.004000663757324219
测试效率: 0.0
```

> 分析下就可以知道，列表是将0-99999都生成后放进列表里了，所以用时比较多。 
而生成器只是封装了算法，每次调用在去调用算法，这样做就可以做到节省内存了。

