---
title: Python028生成器
date:  2018-06-22 18:10:16
tags: python
---

### 生成器

> 根据规则生成你要的东西，在你实际需要的时候生成

```
a = [x*2 for x in range(10)]
# [0,2,4,6,8,10,12,14,16,18]
```

有的时候你需要一个很大的列表这样内存就会需要很多，但是不希望现在就生成，而是使用其中某些个

```
# 基本会死掉
a = [x*2 for x in range(100000000000000000)]
```

> 生成器如何表达

#### 生成器生成方式一

```
原来的方式(列表生成式)
a = [x*2 for x in range(10)]
# [0,2,4,6,8,10,12,14,16,18]

生成器的方式  ==> 将列表生成式的[]改成 ()
b = (x*2 for x in range(10))
# <generator object <genexpr> at 0x79898312312>
```

如何使用生成器里的值

```
next(b) # 0
next(b) # 2
next(b) # 4
next(b) # 6
next(b) # 8
next(b) # 10
next(b) # 12
next(b) # 14
next(b) # 16
next(b) # 18
next(b) # 异常 如果生成器已经到了最后一个值就罢工了。。。直接异常了
```

#### 生成器生成方式二

- yield 执行到它的时候,creatNum执行就结束了,返回 yield 后的值

> 如果一个函数里有yield那么 它返回的就是生成器对象

```
def creatNum():
    print('start')
    a,b = 0,1
    for i in range(5)
        yield b
        a,b = b,a+b
    print('stop')

# 创建了一个生成器对象   creatNum()已经不像函数了，而是一个生成器对象
a = creatNum()

# 第一种方式：使用循环执行迭代器
for num in a:
    print(num)

'''
第二种方式：
两种next方式执行生成器 前提是你要指定生成的次数
res = a.__next__()
res =  next(a)
'''
```

> 详解 yield

```
def creatNum():
    print('start')
    a,b = 0,1
    for i in range(5)
        yield b
        a,b = b,a+b
    print('stop')

a =  creatNum() # 生成器对象

next(a)
# 此时 a是生成器对象  但是是第一次执行 所以就会执行
    '''
    初始化部分
    print('start')
    a,b = 0,1
    然后执行循环第一次  返回yield 后的值 b (b = 1)
    函数停止在 yield
    '''
# 第二次调用 next
    '''
    会从上次停止的地方执行 也就是 yield b (b = 1) 开始
    a,b = b,a+b
    然后根据循环条件看是否继续循环
    发现yield b (b = 1)
    '''
next(a)

# 第三次同第二次的结果 。。。
# 第四次同第二次的结果 。。。
# 第五次同第二次的结果 。。。 由于循环已经结束 所以最后会打印 stop

```

#### send

先看如下程序

```
def test():
    i = 0
    while i<5:
        temp = yield i
        print(temp)
        i += 1

t = test()

a = next(t) #  a为0

a = next(t) #  打印 None ,a为1 

a = next(t) #  打印 None ,a为2
```

> 为啥是None

- 如果 yield 前有如下形式 temp = yield i,并不是把 yield值给了temp 而是在下一次执行生成器的时候给它传递一个值
- 之所以为None 因为 yield i 的时候函数就停止了 temp没有执行到
```
temp = yield i 
要看为两部分  左边 temp  和右边 yield i 
程序执行的时候会从右往左
但是yield i的时候就结束返回了 
```

> 再看

```
def test():
    i = 0
    while i<5:
        temp = yield i
        print(temp)
        i += 1

t = test()
a = next(t) #  a为0
a = next(t) #  打印 None ,a为1 
a = t.send('haha') # 打印 haha ,a 为2

'''
t.send('haha') 是把 haha 作为 yield i 的返回结果赋值为temp
'''
```

##### send 和 next的区别

- 相同点：都可以让生成器向下走一步
- 异同点： send可以给一个值，这个值作为 yield i 的整体的结果

##### 使用send的注意点

```
def test():
    i = 0
    while i<5:
        temp = yield i
        print(temp)
        i += 1

t = test()

t.send('haha') # 报错

##########################################
因为第一次从程序开头执行
即i = 0执行 但是 send('haha')是把值给 yield i的整体的返回结果

解决方式就是 第一次先调用 一次 t.next() 然后在 t.send('haha')

第二种方式 if/else

def test():
    i = 0
    while i<5:
        if i == 0:
            temp = yield i
        else:
            yield i
        i += 1
```

#### 生成器的应用

相当于一起执行(就像单核的CPU进程里时间片的概念看上去执行多个应用实际上是假象)

> 多任务应用

```
def test1():
    while True:
        print('--1--')
        yield None

def test2():
    while True:
        print('--2--')
        yield None

t1 = test1()
t2 = test2()

while True:
    t1.__next__()
    t2.__next__()
```

