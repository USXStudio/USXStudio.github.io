---
title: Python027动态添加属性及方法
date: 2018-06-22 17:10:16
tags: python
---

### 动态添加属性

- 给实例对象添加
- 给类添加

```
class Person(object):
    def __init__:(self,name,age)
        self.name = name
        self.age = age


a1 = Person("a1",100)
print(a1.name)
print(a1.age)
# 给a1添加属性
a1.addr = 'beijing'
print(a1.addr)

b1 = Person("b1",1000)
print(b1.name)
print(b1.age)

# print(b1.addr) 报错 因为 addr是a1对象动态添加的属性，仅跟对象绑定


# 类属性
Person.num = 100

print(a1.num) #100
print(b1.num) #100

```

### 动态添加方法

- 实例方法
- 静态方法
- 类方法

```
class Person(object):
    def __init__:(self,name,age)
        self.name = name
        self.age = age
    def eat(self):
        print("%s正在吃"%self.name)

# run不在 Person类中定义
def run(self):
     print("--------%s正在跑-----"%self.name)

p1 = Person('p1',10)
p1.eat()

#错误方式1
# p1.run()  报错

#错误方式2 (把方法地址传递给 p1.run属性)
#虽然p1对象中，run属性已经指向了 外部 run方法的地址
#因为run属性指向的函数是后来添加的,执行p1.run()的时候,并没有把p1当作第一个参数,所以在调用的时候出现却缺少参数
# p1.run = run 

```

正确的做法是

```
import types

'''
types.MethodType(method,instance) 
可以实现对对象添加动态方法
'''

p1.run = types.MethodType(run,p1) 
p1.run() # 不报错 ！！！
# p1正在吃
```

> types.MethodType深入

如果我们这样发现可以直接调用

```
xxxx = types.MethodType(run,p1) 
xxxx()
# p1正在吃
```

1.  types.MethodType(run,p1) 实际是把实例对象传递到普通方法run里缺少的self参数
2.  types.MethodType(run,p1) 返回的实际就是那个传递了参数的run方法的引用
    ```
        def run(self):
            print(self.name)

         def run(p1):
            print(p1.name)
    ```
#### 静态方法添加

```
@staticMethod
def test():
    print('-----static method ------')


# 静态方法可以直接添加因为没有self参数
Person.test = test;

Person.test()
```

#### 类方法

```
@classMethod
def test2(cls):
    print('-----class method ------')

Person.test2 = test2

Person.test2()

```

### slots作用

因为python是动态语言所以在实例生成之后仍然可以额外添加属性

> 如何做到让它添加指定的属性呢

slots

```
class Person(object):
    __slots__ = ("name","age")


p1 = Person()
p1.name = 'aa' # 正常
p1.age = 20 # 正常
p1.num =1000 # 报错

```