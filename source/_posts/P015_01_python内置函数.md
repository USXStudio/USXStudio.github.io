---
title: P015_01_python内置函数
date: 2018-07-26 18:46:33
tags: fullstack
---

### 什么是内置函数

就是不用你def就可以直接调用的 如 

- print()
- input()
- len()
- type()
- open()
- tuple()
- list()
- int()
- bool()
- set()
- dir()
- id()
- str()

#### 作用域相关

- locals()
- globals()

```
global 变量
nonlocal 变量
```

#### 迭代器/生成器相关

- range
- next
- iter

> next()

```
迭代器.__next__() 这个是迭代器的方法
next(迭代器) 这个是内置函数

它俩完成的事一样，而__next__()是真正完成功能的地方
------------------------------
# next()的实质就是
def next(迭代器):
    迭代器.__next__()

# len()的实质就是
def len([]):
    [].__len__()
```

> iter()

```
迭代器 = iter(可迭代的)
迭代器 = 可迭代的.__iter__()
```

> range()

```
print('__next__' in dir(range(10))) # False 可迭代的，但不是一个迭代器

print('__next__' in dir(iter(range(10))) # True 通过iter转换为迭代器
```

#### 其他

> dir

查看一个变量拥有的方法

```
dir([])
dir({})
dir(1)
```

> callable 

调用相关,我们知道函数调用要使用()才可以,但是我们怎么知道一个xxx()就可以调用呢？

```
print(callable(print)) # True

a = 1
print(callable(a)) # False

```

#### help

> 查看帮助文档(在一些政府，银行禁网的封闭开发情况下)

```
help(str) # 打印和 str相关的方法 以及用法
```

#### 模块相关

- import

```
import time # 表面是调用了一个关键字
time.time()
实际上是
time = __import__(time)
time.time()
```

#### 文件相关

- open

```
f = open('1.txt')
print(f.writeable())  # 代表能不能写入
print(f.readable())  # 代表能不能读取
```

#### 内存相关

- id()  获取变量的内存地址
- hash() 和内存相关的东西

```
print(hash(12345))  # 有结果   
print(hash('fsdafsadfasdf'))   # 有结果
print(hash((1,'aaa')))   # 有结果

print(hash([1,2,3]))  # 报错 unhashable type:'list' 意思就是不可哈希

```
> 对于可哈希数据类型的hash值在一次程序执行过程中总是不变的

如出现两次 'hahaha' 他俩哈希值相同

> #### hash函数和字典的存储规则有关系

字典的查找速度特别快，只要有key可以瞬间找到值

```
原理：
step001
dict = {
    key:value
}
第一次创建key的时候 会生成一个hash值 3214141244(标识一个内存地址)
然后把value放到这个地址里

最后查询 dict[key] 实际是 hash(key) ==> value
```

#### 输入输出

- input
- print

> input

```
res = input('请输入账号') # 输入成功后按回车才把值给 res
```

> print 其实你不会

```
print() # 每次print会自动给你一个换行

不想要换行咋办

print('我们是祖国的花朵',end='')
print('我们是祖国的花朵',end='')
# 我们是祖国的花朵我们是祖国的花朵

print(1,2,3,4,5) # 1 2 3 4 5

有分隔符的输出

print(1,2,3,4,5,sep='|') # 1|2|3|4|5

将输出写入到文件
f = open('file','w')
print('aaa',file=f)
f.close()
```
