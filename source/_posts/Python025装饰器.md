---
title: Python025装饰器
date: 2018-06-22 14:58:16
tags: python
---

### 装饰器

> 不影响你正常工作写代码，但是应试的时候如果你不会就代表你不深入python

先看下面一段代码，(理解函数的覆盖)

```
def t():
    print(1)

def t():
    print(2)

t # 代表函数t的内存地址

t() # 执行函数  打印 2

################################################

def xxx(a):
    print(a)

xxx = lambda x:x+1

xxx(1) # 打印2 被匿名函数覆盖
```

> 重现公司场景

```
############基础平台提供功能############################
def f1():
    print(1)
def f2():
    print(2)
def f3():
    print(3)
def f4():
    print(4)
#############业务部门A###########################
f1()
f2()
f3()
f4()
#############业务部门B###########################
f1()
f2()
f3()
f4()
```

> 公司发展后发现需要职责分离每个部门分配的功能调用不一样

- 如A部门只能调用 f1 f2
- 如B部门只能调用 f3 f4

然后交给你实现一个鉴权功能,你修改后如下

```
def check_login():
    #验证1
    #验证2
    #验证3
    pass

def f1():
    check_login()
    print(1)
def f2():
    check_login()
    print(2)
def f3():
    check_login()
    print(3)
def f4():
    check_login()
    print(4)
```

> 最后部门老大说要遵循「开放封闭」原则，最后给了一个代码的实现

- 封闭：已实现的功能(不该进行更改功能代码的内部)
- 开放：对扩展开发(改外面)

```
def w1(func):
    def inner():
        #验证1
        #验证2
        #验证3
        func()
    return inner

@w1
def f1():
    print(1)
@w1
def f2():
    print(2)
@w1    
def f3():
    print(3)
@w1
def f4():
    print(4)


##########################################
@w1是一个语法糖

@w1
def f1():
    print(1)
实际上代码是这样
def w1(func):
    def inner():
        print('鉴权通过')
        func()
    return inner
程序执行到 @w1的时候 
##########################################
#第一步理解
innerFunc = w1(f1)
innerFunc()
##########################################
#第二步理解——原理
f1 = w1(f1)
f1()
```

#### 装饰器的应用

> 包裹数据给字符串加上 b标签

```
#定义函数：完成包裹数据
def makeBold(fn):
    def wrapped():
        print("----1---")
        return "<b>" + fn() + "</b>"
    return wrapped

@makeBold
def test3():
    print("----3---")
    return "hello world-3"

ret = test3()
print(ret)
```

> 思考？多个装饰器的会如何处理

```
#定义函数：完成包裹数据
def makeBold(fn):
    def wrapped():
        print("----1---")
        return "<b>" + fn() + "</b>"
    return wrapped

#定义函数：完成包裹数据
def makeItalic(fn):
    def wrapped():
        print("----2---")
        return "<i>" + fn() + "</i>"
    return wrapped

@makeBold
@makeItalic
def test3():
    print("----3---")
    return "hello world-3"

ret = test3()
print(ret) # <b><i>hello world-3</i></b>

###########原理############
1.因为此时还存在装饰器所以不会立刻执行test3的返回结果  return "hello world-3"
2.此时返回的是
    def wrapped():
            print("----1---")
            return "<b>" + fn() + "</b>"
test3 = makeBold(test3)
发现可以把  wrapped 返回了
test3 = makeItalic(test3)

```

#### 装饰器执行顺序

```
def w1(func):
    def inner():
        #验证1
        #验证2
        #验证3
        func()
    return inner

@w1
def f1():
    print(1)

```

>  @w1这一行的时候 只要python解释器执行到这里，就自动进行装饰，而不是等调用的时候才装饰

> 如果是多个装饰器

```
@w1
@w2
实际是
1.w2包装了一下
2.w1继续包装了一下


调用的时候
先拆开w1
再拆开w2
```

#### 装饰器对带有参数的函数的处理

```
def w1(func):
    def inner(a,b):
        func(a,b)
    return inner

@w1
def f1(a,b):
    print(a+b)

f1(1,2) # 实际调的是inner(1,2)
```

如何做到接受任意参数的函数的装饰器

```
def w1(func):
    def inner(*args,**kwargs):
        func(*args,**kwargs)
    return inner

@w1
def f1(a,b):
    print(a+b)
```

#### 带返回值的装饰器

> 又来了一个坑如果f1带有返回值装饰器返回的是什么

```
def w1(func):
    def inner():
        func()
    return inner

@w1
def f1():
    print('test')
    return 'haha'

f1() # None
```

再看

```
def w1(func):
    def inner():
        print('1')
        x = func() # 保存返回值 'haha'
        print('2')
        return x
    return inner

@w1
def f1():
    print('test')
    return 'haha'

f1() # None
```

#### 通用装饰器

```
def w1(func):
    def inner(*args,**kwargs):
        x =func(*args,**kwargs)
        return x
    return inner

@w1
def f1(a,b):
    return a+b
```

#### 带参数装饰器

> 装饰器是一次闭包，如果装饰器带参就在进行一次闭包

```
def func_arg(arg):
    def func(functionName):
        def func_in():
            print("---记录日志-arg=%s--"%arg)
            if arg=="heihei":
                functionName()
                functionName()
            else:
                functionName()
        return func_in
    return func

#1. 先执行func_arg("heihei")函数,这个函数return 的结果是func这个函数的引用
#2. @func
#3. 使用@func对test进行装饰
@func_arg("heihei")
def test():
    print("--test--")

#带有参数的装饰器,能够起到在运行时,有不同的功能
@func_arg("haha")
def test2():
    print("--test2--")

test()
test2()

```
