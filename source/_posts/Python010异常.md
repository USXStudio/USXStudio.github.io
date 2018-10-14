---
title: Python010异常
date: 2018-06-18 14:59:50
tags: python
---

### 异常

> try ... except...

```
try:
    print(a) #a没有定义直接打印报错
except:
    print('hahahaha')

# 打印 hahahaha
```

> 异常有多种应该怎样捕获

```
try:
    xxxxxxxxxx1
    xxxxxxxxxx2
    xxxxxxxxxx3
    xxxxxxxxxx4
except 车胎扎了:
    补胎
except 链子掉了:
    挂上
except 闸坏了:
    换车闸

```

> 一次捕获多个异常

```
try:
    xxxxxxxxxx1
    xxxxxxxxxx2
    xxxxxxxxxx3
    xxxxxxxxxx4
except (车胎扎了,链子掉了,闸坏了):
    处理

# python2 里不用加括号
```

> 打印错误的基本信息

```
try:
    print(a)
except NameError as ffff:
    print('产生了一个异常%s'%ffff)

# 多种异常
try:
    print(a)
except (NameError,FileNotFoundError) as ffff:
    print('产生了一个异常%s'%ffff)
```

> 捕获所有异常

```
try:
    print(a)
except:
    print('产生了一个异常')

try:
    print(a)
except Exception as ffff:
    print('产生了一个异常%s'%ffff)
```

#### 异常之else

```
try:
    #如果产生了一个异常，但是except没有捕获，那么就会按照这个异常默认的处理方式进行
    open("aaa.txt")
    a = 100
    print(a)
except NameError:
    #如果在try中的代码产生了一个NameError时，才会执行的异常处理
    print("捕获到了一个异常")
else:
    #在try中的代码都没有产生异常的时候，才会执行的代码
    print("-----1-----")

```

#### 异常之finally

```
try:
    #如果产生了一个异常，但是except没有捕获，那么就会按照这个异常默认的处理方式进行
    open("aaa.txt")
    a = 100
    print(a)
except NameError:
    #如果在try中的代码产生了一个NameError时，才会执行的异常处理
    print("捕获到了一个异常")
else:
    #在try中的代码都没有产生异常的时候，才会执行的代码
    print("-----1-----")
finally:
    print("-----最后执行的实行-0----")
```

#### 异常的传递

```
def test1():
    print("------1-1------")
    print(num)
    print("------1-2------")


def test2():
    print("------2-1------")
    test1()
    print("------2-2------")

def test3():
    try:
        print("------2-1------")
        test1()
        print("------2-2------")
    except Exception as result:
        print("捕获到了异常:%s"%result)


test3()

```

#### 自定义异常

```
class Test(Exception):
    def __init__(self, length, atleast):
        self.length =  length
        self.atleast = atleast

try:
    raise Test(1,2)
except Test as result:
    print("---f-f-f-f-f----")
    print(result.length)
    print(result.atleast)
```