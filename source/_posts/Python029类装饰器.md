---
title: Python029类装饰器
date:  2018-06-23 10:10:16
tags: python
---

### 类装饰器

#### 把类的实例对象当作方法调用

一般类的实例对象是不能直接当作方法调用的，因为方法通常定义在类中

大部分情况是通过 该实例对象 t.fn()的形式调用

> 我们的需求是直接把类的实例对象当作方法调用

```
class Test():
    def fn(self):
        print('fn exe')
t = Test()
t.fn()
t() # 现在会报错！！！ 
#这样调用实际是可以实现的
```

如何实现 t() 的方式调用呢？

```
class Test():
    def __call__(self):
        print('--test--')

t = Test()
t() # --test--
```

#### 类装饰器的实现

```
class Test():
    def __init__(self,fn):
        print('--初始化--')
        print('fn name is %s'@fn.__name__)
        self.__fn = fn
    def __call__(self):
        print('--装饰器中的功能--')
        sefl.__fn()

@Test
def test():
    print('--test--')


test()
'''
    理解
    1. 执行到装饰器及装饰的方法后 也就是从@Test到 test()定义结束  时候相当于构造了类Test的实例对象
        也就是 Test(test) 
        然后执行 init 方法 并且把传递进入的 fn(也就是test) 挂载到实例对象
        此时test 的指向改变为 生成的实例对象
        test = Test(test)
    2. test() 后的理解
        此时test实际上是 Test(test)生成的实例对象
        因为我们声明了 call方法
        所以 test() 执行了 call方法的调用
'''
```