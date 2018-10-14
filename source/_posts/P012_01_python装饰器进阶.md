---
title: P012_01_python装饰器进阶
date: 2018-07-24 12:27:01
tags: fullstack
---

### 先来看看一些题外话(一个函数的相关信息)

- name 函数名
- doc 函数文档

```
def xxx():
    '''
    一个打印 哈哈哈 的函数
    '''
    print('哈哈哈')

print(xxx.__name__)  # xxx
print(xxx.__doc__)   # 一个打印 哈哈哈 的函数
```

> 那要是被装饰器的函数呢？



```
def wrapper(func):
    def inner(*args,**kwargs):
        res = func(*args,**kwargs)
        return res
    return inner

@wrapper
def xxx2():
    '''
    测试xxx2
    '''
    print('HAHA')

print(xxx2.__name__) # inner
print(xxx2.__doc__)  # None
```

#### wraps模块

> 解决查看函数相关信息的问题

```
# step1引入wraps模块
from functools import wraps

def wrapper(func):
    # step2 使用这个模块的装饰器函数
    @wraps(func)
    def inner(*args,**kwargs):
        res = func(*args,**kwargs)
        return res
    return inner

@wrapper
def xxx2():
    '''
    测试xxx2
    '''
    print('HAHA')

print(xxx2.__name__) # xxx2
print(xxx2.__doc__)  # 测试xxx2
```

### 带参数装饰器

通过布尔值来决定「是否」启动装饰器

> 首先装饰器的原理是闭包

> 控制装饰器是否开启的原理就是：在原装饰器的基础上再一次的闭包

```
import time
FLAGE = False # 决定装饰器是否开启的布尔值
def timmer_out(flag):  #接受参数的装饰器用来处理是否开启装饰器
    def timmer(func):
        def inner(*args,**kwargs):
            if flag:
                start = time.time()
                ret = func(*args,**kwargs)
                end = time.time()
                print(end-start)
                return ret
            else:
                ret = func(*args, **kwargs)
                return ret
        return inner
    return timmer
    
# timmer = timmer_out(FLAGE)
@timmer_out(FLAGE)    #wahaha = timmer(wahaha)
def wahaha():
    time.sleep(0.1)
    print('wahahahahahaha')

@timmer_out(FLAGE)
def wahaha2():
    time.sleep(0.1)
    print('wahahahahahaha2222222')

wahaha()
wahaha2()
```

### 多个装饰器调用顺序

```
def wrapper1(func):
    def inner1():
        print('wrapper1 ,before func')
        ret = func()
        print('wrapper1 ,after func')
        return ret
    return inner1

def wrapper2(func):
    def inner2():
        print('wrapper2 ,before func')
        ret = func()
        print('wrapper2 ,after func')
        return ret
    return inner2

def wrapper3(func):
    def inner3():
        print('wrapper3 ,before func')
        ret = func()
        print('wrapper3 ,after func')
        return ret
    return inner3

@wrapper3
@wrapper2
@wrapper1
def f():
    print('in f')
    return '哈哈哈'

print(f())

结果：
wrapper3 ,before func
wrapper2 ,before func
wrapper1 ,before func
in f
wrapper1 ,after func
wrapper2 ,after func
wrapper3 ,after func
哈哈哈
```

> 多个装饰器的效果就是「俄罗斯套娃」


