---
title: P016_01_python内置函数收尾
date: 2018-07-26 21:54:31
tags: fullstack
---

### python中一个让你懵的面试题

> 数据类型有哪些？

- int
- bool

> 数据结构

- dict
- list
- tuple
- set
- str

### 剩余内置函数

> reversed() 保留原列表，返回一个反向的迭代器

```
l = [1,2,3,4,5]
l.reverse()
print(l) # 此时 l 还是那个列表只不过内容反序了

l = [1,2,3,4,5]
l2 = reversed(l)  # 保留 l 不变 ，返回一个反序的迭代器
print(l2)
```

> slice() 切片方法

```
l = (1,2,23,213,5612,342,43)
sli = slice(1,5,2)
print(l[sli])
print(l[1:5:2])
```

#### 字符串相关内置方法

> format()

```
print(format('test', '<20')) # 开辟20个字符空间 左对齐
print(format('test', '>40')) # 开辟40个字符空间 右对齐
print(format('test', '^40')) # 开辟40个字符空间 居中对齐

结果：
test                
                                    test
                  test 
```

> bytes() 把unicode转换成 「对应编码」 的bytes

```
# 我拿到的是gbk编码的，我想转成utf-8编码
print(bytes('你好',encoding='GBK'))     # unicode转换成GBK的bytes
print(b'\xc4\xe3\xba\xc3'.decode(encoding='GBK'))
print(bytes('你好',encoding='utf-8'))   # unicode转换成utf-8的bytes
print(b'\xe4\xbd\xa0\xe5\xa5\xbd'.decode(encoding='utf-8'))
结果：
b'\xc4\xe3\xba\xc3'   gbk编码的byte编码
你好    decode按照gbk解码后的unicode字符
b'\xe4\xbd\xa0\xe5\xa5\xbd'   utf-8编码的byte编码
你好    decode按照utf-8解码后的unicode字符
```

> bytearry()

- 网络编程 只能传二进制
- 照片和视频也是以二进制存储
- html网页爬取到的也是编码

```
b_array = bytearray('你好',encoding='utf-8') # unicode 你好转换为 utf-8编码的 byte数组
print(b_array)
print(len(b_array))
print(b_array[0])

结果：
bytearray(b'\xe4\xbd\xa0\xe5\xa5\xbd')
6
228
```

> memoryview (非重点知识点)

- 将数据转换为指定编码的byte之后进行一次memoryview就可以进行切片,同时：这个切片不创建新的,而且这个切片是字节类型的

[详情请参考 https://segmentfault.com/q/1010000007137721](https://segmentfault.com/q/1010000007137721)

也可以看我的摘录上述链接的信息在 [P016_01_python内置函数收尾]

> ord() 字符按照unicode转数字

```
print(ord('好')) # 22909
print(ord('a'))  # 97
print(ord('1'))  # 49
```

> chr() 将数字转字符

```
chr(97) # a
```

> ascii() 

只要是ascii码中的内容，就打印出来，不是就转换成\u


> #### repr() 原样输出

用于%r格式化输出(%r实际就是调用repr())

```
name = 'egg'
print('你好%r'%name)
print(repr('1'))
print(repr(1))

结果：
你好'egg'
'1'
1
```

#### 数据集合相关方法

1. 字典：

- dict

2. 集合：

- set：无序排序且不重复，是可变的。
- frozenset：是冻结的集合，他是不可变的，存在哈希值。可以作为字典的key

#### 判断值的False/True

- all
- any

```
all(['a','',123]) # False 有一个值是False就是False

any(['a',123]) # True 
any(['a','',123]) # True 有一个值是True就是True
```

#### 拉链方法

- zip

```
a = [1,2,3]
b = ['a','b','c']
# zip(a,b) # 返回的是 迭代器
for i in zip(a,b):
    print(i)

结果：
(1,'a')
(2,'b')
(3,'c')
```

> 如果俩个列表长度不一致呢？

从头开始对齐，以短列表长度为准

```
a = [1,2,3,4]
b = ['a','b','c']
for i in zip(a,b):
    print(i)

结果：
(1,'a')
(2,'b')
(3,'c')
```

> 多个呢？而且是元组呢？元组子元素继续是列表呢？

```
a = [1,2,3,4]
b = ['a','b','c']
c = ('*','**',[1,2])
for i in zip(a,b,c):
    print(i)

结果：
(1,'a','*')
(2,'b','**')
(3,'c',[1,2])
```

> 如果是字典呢？

```
a = [1,2,3,4]
b = ['a','b','c']
c = ('*','**',[1,2])
d = {"k1":1,"k2":2}
for i in zip(a,b,c,d):
    print(i)

结果：
(1,'a','*','k1')
(2,'b','**','k2')
-----------------------------由于字典的key是无序的 所以 k1 k2的顺序不是固定的
```

#### filter和map

> filter(函数名,可迭代的数据类型)

根据返回值的布尔值来过滤

```
def is_odd(x):
    return x%2 == 1

res = filter(is_odd,[1,4,6,7,9,12,17])
print(res) # 又是一个迭代器

for i in res:
    print(i)
结果：
1
7
9
17
```

> 练习：求1-100开平方是100的数字

```
#要引入模块了
form math import sqrt
def func(n):
    res = sqrt(n)
    return res%1 == 0 

res = filter(func,range(1,101))
```

> map

```
res = map(abs,[1,-4,6,-8])
for i in res:
    print(i)
结果：
1
4
6
8
```

> 总结:

- filter执行之后的结果集合 <= 数据源
- map 执行之后元素个数不变,值可能发生改变

#### sorted函数

可定制的排序

- 生成了一个新列表
- 不改变原列表
- 占内存

```
a = [1,-4,6,5,-10]
a.sort(key=abs) # 在原列表基础上进行排序

b =  [2,-4,6,-5]
print(sorted(b)) # 直接返回排序后的列表（已经不是生成器了，所以慎用），它排序后原列表还在
print(b)
```

> 花式用法

```
sorted(a,reversed=False)
sorted(a,reversed=True)
sorted(a,key=abs,reversed=False)
```

> 练习：列表按照每一个元素的长度排序

```
a = ['   ',[1,2],'hello world']
new_a = sorted(a,key=len)

你也可以这样
def xxx(n):
    return len(n)
new_a = sorted(a,key=xxx)
```