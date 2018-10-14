---
title: Python006面向对象之我不是异教徒
date: 2018-06-16 22:34:49
tags: python
---

稍微懂点js，而且js里的继承是模拟的即使出了ES6 class特性但是它的实质依然是原型链具体请参考之前写的总结文章「继承」

### 面向对象

> 什么是面向对象，最深刻的理解莫过于think in java里的灯泡图

虽然今天是python

- 面向过程

1. 老张开车去东北
2. 先搞辆车
3. 充满油
4. 踩上油门
5. 打开百度地图
6. 东北之旅

- 面向对象

1. 打开支付宝飞猪订票 or 火车12306
2. 我去！！！竟然莫名其妙的就来到了东北

### 类与对象

- 类 ---> 同类事物的抽象描述
- 对象 ---> 具体的事物

```
类    对象

狗    张三家的萨摩

车    邻居家的小汽车
```

### 类的构成

- 类名 狗 Dog
- 属性 品种、毛色、性别、名字
- 方法 叫、跑、咬人

### 定义一个类

```
Class Cat:
    #属性

    #方法 
    '''
    只要是方法要加入 self参数 
    不管有没有参数 self为第一个参数
    def原来是顶格写 在类里要 缩进一下
    '''
    def eat(self):
        print('吃鱼啦！！！')


# 创建一只猫
a = Cat()
a.eat()
```


>  给猫添加属性

```
a.color = '花色'
# 获取a对象的数据
xxx = a.color  # '花色'

# 注意如果没有的属性你还去获取会产生异常
print(a.high) # 报错
```


> 如果想在方法里访问 color属性就要这样定义，否则抛出异常

```
Class Cat:
    #方法 
    def xxx(self):
        print(self.color)

b = Cat()
# 给对象添加color属性
b.color = 'red'
# 通过方法访问color属性 
'''
但是这是一个大坑
必须先声明b.color = 'red' 否则还是报错
因为color是你动态添加的属性
'''
b.xxx() # 'red'
```

### init方法(初始化方法)

创建对象后自动调用

```
class Cat:
    # 当创建完一个对象后会立即调用
    def __init__(self):
        print('haha')
    
    def eat(self):
        print('吃鱼啦！！！')

a = Cat()
# 'haha'
```

稍微好点的挂载属性

```
class Cat:
    # 当创建完一个对象后会立即调用
    def __init__(self,newColor,newHigh):
        self.color = newColor
        self.high = newHigh
        print('haha')
    
    def eat(self):
        print('吃鱼啦！！！')

a = Cat('red',50)
# 'haha'
print(a.color)
print(a.high)
```

### 扩展-创建对象的浪费之处

```
a = Cat()
b = Cat()

'''
a对象里有一个 eat方法
b对象里有一个 eat方法
如果是100个对象就是100个 eat方法 很浪费因为方法的内容都是一样的
'''
```

### str方法(神似js的toString实现)

```
class Cat:
    def __str__(self):
        return 'haha'

# 这样打印对象的时候就会变成 'haha'
a = Cat()
print(a) # 'haha'
```

### 私有属性

目的就是保证属性的私有，不让对象的操作者随意操作属性

```
class Person:
    def __int__(self,name,age):
        self.name = name
        self.age = age

laowang = Person("老王",30)
# 属性不私有 就会造成随意篡改
laowang.age -= 1



class Person:
    def __int__(self,name,age):
        self.name = name
        self.age = age
    def setNewAge(self,newAge):
        self.age = newAge

laowang = Person("老王",30)
# 属性应该有set/get 方法
laowang.setNewAge(300)
# 但是通过 laowang.age -= 1 调用依然可以操作
```

> 私有属性

- 所谓私有，就是不能在外部使用
- 原来没有添加__的属性，默认是公有
- 不管是属性还是方法在前面加上 __代表私有外面就无法调用

```
class Person:
    def __int__(self,name,age):
        self.name = name
        self.__age = age
    def setNewAge(self,newAge):
        self.__age = newAge

laowang = Person("老王",30)
laowang.setNewAge(300)
#通过 laowang.__age -= 1 调用报错
# print(laowang.__age) 报错
```

> 就是任性的想调用私有方法可以吗？ 可以，间接的调用

```
class Person:
    def __int__(self,name,age):
        self.name = name
        self.__age = age
    def setNewAge(self,newAge):
        self.__age = newAge
    def __test(self):
        print('__test')
    def test2(self):
        self.__test()
        print('test2里调用 __test')
```

> 虽然无法调用私有方法和属性，但是可以查看私有的东西

```
dir(类名)

打印该类私有的属性和方法
如果是你自己定义的私有方法__test
会被改名为 _Test__test

隐姓埋名。。。
```

### del方法

当创建一个对象时会默认调用init

当删除一个对象时也会默认调用一个方法就是 del

```
class Person:
    def __int__(self,name,age):
        self.name = name
        self.__age = age
    def __del__(self):
        print('------del-------')

a = Person("xxx",30)
```

- python程序结束会调用del
- 手动删除对象del a 也可以触发del方法调用

> 如果手动多次del a 程序会挂掉

> 思考一个问题

```
a = Person("xxx",30)
# 把a的地址给了 b 
b = a

# a和b都指向同一块内存区域(堆) 引用计数为2

del a
#删除了a的引用 并不调用del方法()  引用计数为1
print(b) #正常

del b 
#删除了b的引用 并调用del方法  引用计数为0
```