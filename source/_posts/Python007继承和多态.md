---
title: Python007继承和多态
date: 2018-06-17 12:15:42
tags: python
---

### 继承

记得有句经典话，你可以使用你爸的抽屉，但你只有使用权，而没有抽屉所有权，因为抽屉是你爸的

> 所有类默认继承 object(object是所有类的基类)

```
class Cat:
    pass

class Cat(object):
    pass

#两种方式一样 默认继承object
```

#### 单继承

```
class Cat:
    def run(self):
        print('----run---')

class Bosi(Cat):
    pass

class Jiafei(Cat):
    pass

bosi = Bosi()
bosi.run()

jf = Jiafei()
jf.run()
```

> 继承的注意点，后代无法使用父类的私有属性和方法

你的牙刷和你爸的牙刷属于私有的

> 重写的应用

```
class Cat:
    def run(self):
        print('----run---')

class Bosi(Cat):
     def run(self):
        print('----run RUN RUN---')

# 波斯猫的run方法跟父类不一致的时候可以自行定义行为——重写
```

> 如果想要在重写的方法里调用父类的方法

```
class Cat:
    def run(self):
        print('----run---')

class Bosi(Cat):
     def run(self):
        super().run()
        print('----run RUN RUN---')
```

#### 多继承

- 马驴骡子实例

```
class Ma(object):
    def pao(self):
        print('----100km/h  跑----')

class Lv(object):
    def tuowupin(self):
        print('----托物品----')

# 多继承
class Luozi(Ma,Lv):
    pass

luozi = Luozi()
luozi.pao()
luozi.tuowupin()
```

> 注意点：如果马和驴都有 jiao()骡子继承谁的

```
class Ma(object):
    def pao(self):
        print('----100km/h  跑----')
    def jiao(self):
        print('----马在叫----')

class Lv(object):
    def tuowupin(self):
        print('----托物品----')
    def jiao(self):
        print('----驴在叫----')
# 多继承
class Luozi(Ma,Lv):
    pass

luozi = Luozi()
luozi.jiao() #是谁的？
```

> #### 如果继承多个类，每个类都有同名方法

- 如果是平行关系，则继承写在第一个继承的类

```
# 继承Ma的jiao
class Luozi(Ma,Lv):

# 继承Lv的jiao
class Luozi(Lv,Ma):

```

- 如果继承的有多重嵌套关系，而且又特别复杂

它有一个mro算法,不在本次总结范围之内

但是你看有查看调用类的先后顺序

```
print(Luozi.__mro__)
# 打印出继承类的先后顺序
# 继承时寻找方法会从头往后找，使用最先找到的
```

### 多态

地下城与勇士实例,以下是伪代码

```
class 勇士:
    def 攻击(self):
        print('按X  XX XXX')

class 法师(勇士):
    def 攻击(self):
        print('魔法星弹。。。。')

class 鬼剑士(勇士):
    def 攻击(self):
        print('拔刀斩。。。。')


# 这是一个函数
def fight(obj):
    obj.攻击()


fight(勇士()) # '按X  XX XXX'
fight(法师()) # '魔法星弹。。。。'
fight(鬼剑士()) # '拔刀斩。。。。'
```

- 同样的方法表现方式不一样

> 不是勇士的子类可不可以调用fight？

可以，但是你要保证你自身有 「攻击( )」

```
class 喷火龙:
    def 攻击(self):
        print("喷出火焰。。。。")

fight(喷火龙())
```
