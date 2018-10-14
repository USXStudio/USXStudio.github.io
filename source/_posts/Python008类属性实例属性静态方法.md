---
title: Python008类属性实例属性静态方法
date: 2018-06-17 13:50:27
tags: python
---

### 类属性

```
class 士兵:
    #类属性
    血量 = 100
    def __init__(self,name):
            self.name = name
    #方法
    def run(self):
        print('士兵奔跑')    


a = 士兵('a')
b = 士兵('b')

a.血量  # 100
b.血量  # 100
```

> 如果实例属性里定义了 血量 属性

```
class 士兵:
    #类属性
    血量 = 100
    def __init__(self,name):
            self.name = name
            self.血量 = 200
    #方法
    def run(self):
        print('士兵奔跑')    


a = 士兵('a')
a.血量 # 200
```

### 类方法

> 要用修饰器 @classmethod来标识

```
class Test:
    #类属性
    num = 0
    def __init__(self):
        #实例属性
        self.age = 20
    @classmethod
    def setNum(cls,newNum):
        cls.num = newNum

a = Test()
print(Test.num) # 0

Test.setNum(100) 
print(Test.num) # 100

a.setNum(200)
print(Test.num) # 200

# 类方法既可以通过 类调用 也可以通过 实例对象
```

### 静态方法

> 要用修饰器 @staticmethod来标识

```
class Test:
    def __init__(self):
        #实例属性
        self.age = 20

    @staticmethod
    def test():
        print('我是Test类的静态方法')

a = Test()
a.test()
Test.test()
```
