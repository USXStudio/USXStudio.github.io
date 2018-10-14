---
title: 030Python元类
date: 2018-06-23 12:10:16
tags: python
---

### 元类

python里一切皆对象，所以类也是对象

```
class Test(object):
    print('类也是对象')
    def __init__(self):
        self.name = 'abc'

执行这个程序的时候 会打印  “类也是对象”
```

#### 动态创建类

```
def select_class(name):
    if name == 'a'
        class A(object):
            pass
        return A
    else:
        class B(object):
            pass
        return B

myClass = select_class('a')  

```

#### 深刻理解对象

type函数

```
class Person:
    pass

p1 = Person()
type(p1) # __main__.Person

type(100) # int

type("hello") # str
```

#### 使用tppe创建对象

```
Test2 = type("Test2",(),{})

t2 = Test2()

type(t2) #  __main__.Test2
```

> 类带属性

```
class Test:
    num = 0

Test2 = type("Test2",(),{"num":100})


t = Test()
t.num # 0

t2 = Test2()
t2.num # 100
```

> 类带方法

```
class Test:
    def xx(self):
        print('num is %s'%self.num)


def xx2(self):
    print('num is %s'%self.num)

Test2 = type("Test2",(),{"xx2":xx2})


t = Test()
t.num = 100
t.xx() # num is 100

t2 = Test2()
t2.num = 100
t2.xx2() # num is 100
```

> type里的()参数是干啥的

继承的类

```
class Animal:
    def eat(self):
        print('--eat--')

class Dog(Animal):
    pass

# ()代表一个元组 声明元组如果只有一个元素要多加一个","
Cat = type("Cat",(Animal,),{})
```

### 理解元类(非重点，面试可能会问)

元类就是类的类

- 使用元类创建出一个对象，这个对象叫做 类
- 使用类创建的叫做 实例对象

```
Cat.__class__ # type
Dog.__class__ # type
i_cat.__class__ # __main__.Cat
i_dog.__class__ # __main__.Dog

type.__class__  # type

```

#### metaclass属性

> 自行百度(太偏的知识点，平时机会不用)

```
如果class在定义的时候 设置了 __metaclass__ = 一坨代码
那么 一坨代码 怎么写的 类就怎么定义

class Test(object):
    __metaclass__ = 一坨代码

######################
如果一个类里没有 __metaclass__ 就会去父类找 这个属性
如果父类没有就会使用系统默认的
```