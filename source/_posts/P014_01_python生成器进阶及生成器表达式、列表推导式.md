---
title: P014_01_python生成器进阶及生成器表达式、列表推导式
date: 2018-07-25 14:23:38
tags: fullstack
---

### 生成器回顾

> 双下方法 ： 很少直接调用的方法。一般情况下，是通过其他语法触发的

#### next()

```
#娃哈哈%i
def wahaha():
    for i in range(2000000):
        yield '娃哈哈%s'%i

# wahaha是一个生成器函数
g = wahaha()

#我们获取它的内容明显不该 这样
# g.__next__()

print(next(g))
print(next(g))
print(next(g))
print(next(g))
```

#### send()

> 

```
def generator():
    print(123)
    content = yield 1
    print('=======',content)
    print(456)
    yield 2

# 返回生成器对象
g = generator() 

# 返回第一个yield 后的值 1
ret = g.__next__()
print('***',ret)

# 给上一个  yield传递一个值
ret = g.send('hello')   #send的效果和next一样
print('***',ret)

结果：
123
*** 1
======= hello
456
*** 2
```

#### send和 next的区别

- 相同点：都可以让生成器向下走一步
- 异同点： send 可以给一个值，这个值作为上一个 yield 的整体的结果返回


> 使用send的注意事项:

1. 第一次使用生成器的时候,必须要用next获取下一个值(因为send是给上一个yield 设置结果，而第一次的时候没有上一个yield)
2. 最后一个yield不能接受外部的值(因为后面没有 yield 了)

> 为啥第一次不能用send() 

因为会报错！！！详情请看 [为什么python的yield第一次不能用send发送数据？](https://www.zhihu.com/question/28105502)

#### 经典练习

> 求移动平均值

```
'''
10 => 10
10 20 => 15
10 20 30 => 20
10 20 30 10 => 17.5
avg =sum/count
'''
def xxx():
    sum = 0
    count = 0
    num = 0
    avg = 0
    while True:
        # num = yield # 接受一个10
        sum += num  # 10
        count += 1  # 1
        avg = sum/count
        num = yield avg

avg_g = xxx()

avg_g.__next__()
avg_g.send(10)
avg_g.send(20)
avg_g.send(30)
avg_g.send(10)

结果：
0.0
5.0
10.0
15.0

```

> 移动平均值优化

需求就是不要第一次调用next(),因为对使用者没有意义

提示：装饰器

```
def init(func):
    def inner(*args,**kwargs):
        g = func(*args,**kwargs)
        g.__next__()
        return g
    return inner

@init
def xxx():
    sum = 0
    count = 0
    num = 0
    avg = 0
    while True:
        # num = yield # 接受一个10
        sum += num  # 10
        count += 1  # 1
        avg = sum/count
        num = yield avg

avg_g = xxx()

res = avg_g.send(10)
print(res)
res = avg_g.send(20)
print(res)
res = avg_g.send(30)
print(res)
```

> 这个效果就是 预激活装饰器

#### python3新增知识点

先看如下例子:把a,b里面的字符一个一个的返回

```
def xxx():
    a = 'abc'
    b = '123'

    for i in a:
        yield i
    for i in b:
        yield i

g = xxx()

for i in g:
    print(i)


结果：
a
b
c
1
2
3   
```

添加 yield from

```
def xxx():
    a = 'abc'
    b = '123'

    yield from a
    yield from b

g = xxx()

for i in g:
    print(i)

结果：
a
b
c
1
2
3
```

> #### yield from 的作用就是：

从一个容器类型里取得值不需要一个一个取而是集体返回


### 各种表达式推到式

#### 列表推到式

10个鸡蛋的列表

```
'''
eggList = []
for i in range(10):
    eggList.append("鸡蛋%s"%i)

print(eggList)
'''

# 一句话代替上面的效果
eggList2 = ["鸡蛋%s"%i for i in range(10)] #列表推到式
print(eggList2)


[i for i in range(10)] # [0,1,2,3,4,5,6,7,8,9]

[i*i for i in range(10)] # [0,1,4,9,16,25,36,49,64,81]
```

#### 生成器表达式

```
g = ( i for i in range(10) )
print(g) #注意这是生成器
```

> 列表推导式和生成器表达式的区别

- 括号不一样
- 返回值不同，生成器表达式几乎不占用内存

> 一道面试题

```
g = ( i for i in range(10) )

发生了什么：
1.跟函数定义一样什么也不执行
2.直到 g.__next__() 时，里面的循环才走一次
```

#### 各种推导式

公式

```
[每一个元素 for 元素 in 可迭代数据类型]
[满足条件元素 for 元素 in 可迭代数据类型 if 元素相关条件判断]
```

> 30以内被3整除的数

```
[i for i in range(30) if i%3 == 0]
```

> 30以内被3整除的数的平方

```
[i*i for i in range(30) if i%3 == 0]
```

> 找到多重嵌套列表内名字包含e字母 超过两个的

```
names = [
    ['ae','ee','eve','even'],
    ['xx','xe','egg','ele']
]

[name for lst in names for name in lst if name.count('e')==2 ]
```

> ##### 字典推导式

将一个字典的key和value颠倒

```
xx = {'a':10,'b':22}

xx2 = { xx[k]:k for k in xx}
```

> ##### 集合推导式

计算列表中每个值的平方，在带去重功能

```
squared = {x**2 for x in [1, -1, 2]}
print(squared)
# set([1, 4])
```

### 面试题详解

> 面试题1

```
def demo():
    for i in range(4):
        yield i

g = demo()
g1 = ( i for i in g)
g2 = ( i for i in g1)

print(list(g1))
print(list(g2))

结果：
[0,1,2,3]
[]

前5行啥都没干
直到print(list(g1)) 时才从g中生成到g1 ,但是g1是一个生成器 在list(g1)时把g1里的值已经取走了

所以到print(list(g2))的时候 是个空列表
```

> 面试题2

```
def add(n,i):
    return n+i

def test():
    for i in range(4):
        yield i

g = test()
for n in [1,10,5]:
    g =( add(n,i) for i in g)

print(list(g))

解析：
第一步
n = 1 
g= (add(n,i) for i in g)
n = 10 
g= (add(n,i) for i in g)
n = 5
g= (add(n,i) for i in g)
第二步--》换元法
n = 1 
g= (add(n,i) for i in test())
n = 10 
g= (add(n,i) for i in (add(n,i) for i in test()))
第三步--》换元法
n = 5
g= (add(n,i) for i in (add(n,i) for i in (add(n,i) for i in test())) )
第四步带入 n =5  0,1,2,3
(add(n,i) for i in (add(n,i) for i in (add(n,i) for i in (0,1,2,3))))

n = 5
(add(n,i) for i in (add(n,i) for i in (5,6,7,8)))
n = 5
(add(n,i) for i in (10,11,12,13))

(15,16,17,18)

```