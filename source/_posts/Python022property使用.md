---
title: Python022property
date: 2018-06-20 18:26:49
tags: python
---

### property

> 先看私有属性get和set方法

```
class  Test(object):
    def __init__(self):
        self.__num = 100
    def getNum(self):
        return self.__num
    def setNum(self,newNum):
        self.__num = newNum

# 类 Test有私有属性 num (__属性名，代表私有属性)
# 通过get 和 set方法 可以对私有属性进行操作
t = Test()
print(t.getNum())
t.setNum(50)
print(t.getNum())
```

> 需求来了

- t.num = 100 就调用设置 __num的值为100
- print(t.num)的时候就直接取__num的值

```
class  Test(object):
    def __init__(self):
        self.__num = 100
    def getNum(self):
        return self.__num
    def setNum(self,newNum):
        self.__num = newNum
    num = property(getNum,setNum)
```

```
t.num = 9 相当于 t.setNum(9)
t.num 相当于 t.getNum()
```

如果你在定义类的时候设置了一个私有属性，同时设置了get和set方法，并设置property

- 相当于把方法进行了封装，开发者在对属性设置数据的时候变得方便

#### 设置property的另一种方式

```
class Test(object):
    def __init__(self):
       self.__num = 100

    @property
    def num(self):
        print("----getter----")
        return self.__num

    @num.setter
    def num(self, newNum):
        print("----setter----")
        self.__num = newNum

t = Test()

t.num = 200 #相当于调用了 t.setNum(200)

print(t.num) #相当于调用了 t.getNum()

```

