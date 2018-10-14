---
title: Py003-01-09proterty
date: 2018-09-09 09:37:21
tags: M03
---

### property的使用

房屋面积的需求

```
class Room:
    def __init__(self,long_val,width_val):
        self.long_val = long_val
        self.width_val = width_val
    # 房屋的面积
    def Area(self):
        return self.long_val * self.width_val

r = Room(100,100)

# 我想要房屋的面积数据,但是要通过方法一样调用  不能像 r.long_val一样去获取
print(r.Area())
```

> #### property的使用

- 使使用者感知不到他在调用方法，它以为是在调用属性

添加property装饰器后，像访问属性一样访问数据——本质依然是触发方法Area的执行

```
class Room:
    def __init__(self,long_val,width_val):
        self.long_val = long_val
        self.width_val = width_val
    @property
    def Area(self):
        return self.long_val * self.width_val

r = Room(100,100)
print(r.Area)

# Area能像属性一样赋值吗？
# r.Area = 2000 报错 AttributeError: can't set attribute

```

#### propety的补充(了解就行)

突然你就想像数据一样，给Area赋值呢？ 可以

```
class People:
    def __init__(self,name):
        self.__name = name
    @property
    def name(self):
        return self.__name
    @name.setter
    def name(self,val):
        if not isinstance(val,str):
            print('名字必须是字符串')
            return
        self.__name = val

    @name.deleter
    def name(self):
        print('delete 失败')

p = People('hjx')
print(p.name)
p.name = '李四'
print(p.name)

del p.name
```