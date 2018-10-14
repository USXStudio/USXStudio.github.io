---
title: P011_01_python装饰器初识
date: 2018-07-23 21:16:00
tags: fullstack
---

### 1.零碎知识点

#### 三目运算符

> 公式： 变量 = 条件返回True的结果  if 条件 else 条件返回False的结果

```
a = 1
b = 5
c = a if a>b else b # 三元运算
print(c) # 5
```

#### time模块

- time.time() 
- time.sleep() 让程序执行到这个位置的时候停一会儿

```
import time 
time.sleep(5) # 让程序执行到这个位置的时候停一会儿
print('哈哈哈')

print(time.time()) # 1514426720.953799 返回从1970.1.1 0:0:0到现在的 秒

# 5秒后 打印 哈哈哈
```

### 2.装饰器前奏之年终评级测试代码效率

> 1.测试效率

```
import time 

def test():
    time.sleep(0.1)
    print('老板好同事好大家好')

def timmer(func):
    start = time.time()
    func()
    end = time.time()
    print(end - start)

# 测试代码效率
timmer(test)
```

> 2.这样虽然实现了，但是你的同事必须 每次都timmer(xx) /timmer(aa) /timmer(bb)

我们想要的最终结果就是 直接调用func()

> 需求：不想修改函数的调用方式，但是还想在原来函数前后添加功能

> #### timmer 就是一个装饰器函数，只是对一个函数，有一些装饰作用

```
import time 

def test():
    time.sleep(0.1)
    print('老板好同事好大家好')

def timmer(func):
    def inner():
        start = time.time()
        func()
        end = time.time()
        print(end - start)
    return inner

# 这样的效果就是如下
test = timmer(test)
test()
```

### 3.装饰器初识

> 原则：开放封闭原则

- 开放：对扩展是开放的（在不改变原功能函数调用方式的基础上扩展）
- 封闭：对修改是封闭的（不修改原功能函数）

#### 语法糖

```
import time 

def timmer(func):  #装饰器函数
    def inner():
        start = time.time()
        func()
        end = time.time()
        print(end - start)
    return inner

@timmer          #语法糖 @+装饰器函数
def test():      #被装饰的函数
    time.sleep(0.1)
    print('老板好同事好大家好')

# 直接这样就实现了刚刚的功能
# test = timmer(test) 语法糖就是去掉了这句话
test()

print(test) #None
```

为什么最后 test()的结果是None

- inner没有返回值

修改如下:

```
import time 
def timmer(func):  #装饰器函数
    def inner():
        start = time.time()
        res = func() # 用一个变量保存 func的执行结果
        end = time.time()
        print(end - start)
        return res
    return inner

@timmer         
def test():     
    time.sleep(0.1)
    print('老板好同事好大家好')
    return '老板好'

test() # '老板好'
```

> 虽然我们简单的实现了装饰器的雏形，但是如果功能函数要接受参会素怎么办？

```
import time 
def timmer(func):  #装饰器函数
    def inner(a):
        start = time.time()
        res = func(a) # 用一个变量保存 func的执行结果
        end = time.time()
        print(end - start)
        return res
    return inner

@timmer         
def test(a):     
    time.sleep(0.1)
    print('老板好同事好大家好',a)
    return '老板好'


# 内部原理
# test = timmer(test) 此时 test = inner
# test(1)  inner(1)

test(1)
```

> 如果有两个参数呢？继续傻傻的填参数修改。。。

```
import time 
def timmer(func):  #装饰器函数
    def inner(a,b):
        start = time.time()
        res = func(a,b) # 用一个变量保存 func的执行结果
        end = time.time()
        print(end - start)
        return res
    return inner

@timmer         
def test(a,b):     
    time.sleep(0.1)
    print('老板好同事好大家好',a,b)
    return '老板好'


# 内部原理
# test = timmer(test) 此时 test = inner
# test(1,2)  inner(1,2)

test(1,2)
```

> 我们不知道函数参数有多少或者是什么形式的（万能参数）

```
import time 
def timmer(func):  #装饰器函数
    def inner(*args,**kwargs):
        start = time.time()
        res = func(*args,**kwargs) # 用一个变量保存 func的执行结果
        end = time.time()
        print(end - start)
        return res
    return inner

@timmer         
def test(a,b,c):     
    time.sleep(0.1)
    print('老板好同事好大家好',a,b)
    return '老板好'


# 内部原理
# test = timmer(test) 此时 test = inner
# test(1,2,c=3)  inner(1,2,c=3)

test(1,2,c=3)
```

> #### 去掉所有多余的代码,再看装饰器代码

```
def wrapper(func):  #装饰器函数 func是被装饰的函数
    def inner(*args,**kwargs):
        res = func(*args,**kwargs) # 用一个变量保存 func的执行结果
        return res
    return inner

@wrapper        # test = wrapper(test)         
def test(a,b,c):     
    time.sleep(0.1)
    print('老板好同事好大家好',a,b)
    return '老板好'
```

#### 小结

装饰器

- 开发原则：开放封闭原则
- 装饰器的作用：在不改变原函数的调用方式的情况下，在函数前后添加功能
- 装饰器的本质：闭包函数




