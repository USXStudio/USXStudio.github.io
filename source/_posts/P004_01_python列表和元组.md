---
title: P004_01_python列表和元组
date: 2018-07-22 10:26:17
tags: fullstack
---

### 列表

python里的列表(神似js里的数组)

```
names =['aa','bb','cc']
type(names) # list
```

遍历列表

```
infos = ['a','b','c','d']

for temp in infos:
    print(temp)

i = 0
while i<len(infos):
    print(infos[i])
    i+=1
```

#### 列表操作

> 增

- append(obj) 向列表最后添加一个元素

```
a = [1,2,3]
a.append(4)
# [1,2,3,4]
```

- extend 添加内容必须是一个可迭代对象

```
a = [1,2,3,4]
b = [100,200]
a.extend(b)
# [1,2,3,4,100,200]
```

- insert(index,obj) 指定位置添加

```
b = [1,2,3,4]
b.insert(1,99)
# [1,99,2,3,4]
```

> 查

- in
- not in 

```
#判断列表里是否存在
# 笨方法
# 笨方法
inputName = 2
flg = 0 
names = [1,2,3,4,5]
for name in names:
    if name = inputName:
        flg = 1
        break

# 好方法
if inputName in names:
    print('find')
else:
    print('not find')

```

- index
- count

```
a = [1,1,2,2,3,3,4]
a.index(1,1,3)  # 左闭右开[1,3)

a.count(2)
# 2
```

> 删

- del 根据索引删 (也可以直接删除整个数组)
- pop 删最后一个(也可以传递index参数删除指定位置的元素，不传递默认最后一个)
- remove 根据值删
- clear 清空

```
--------------------------------
# del
a = [1,2,3,4,5,6]
del a[0]
# [2,3,4,5,6]

del a 
print(a) # 报错 在内存里把数组删除了
--------------------------------
a.pop()
# [2,3,4,5]

--------------------------------
# clear
b = ['a','b','c']
b.clear() # []
--------------------------------

> 删除部分(切片删除)

```
a = [1,2,3,4,5]
del a[0:2] # [3,4,5]
```

```

> 改

- 按切片改如果内容是字符串就会把修改的内容迭代添加到该位置

```
a = ['aaa','bbb','ccc']
a[0:2] = 'ab'
print(a) # ['a','b','bbb','ccc']
a[0:2] = 'abcd'
print(a) # ['a','b','c','d','bbb','ccc']
```

- 如果切换改的内容是数组也是迭代添加到指定位置

```
a = ['aaa','bbb','ccc']
a[0:3] = [1,2,3,'a','b']
print(a) # [1,2,3,'a','b','bbb','ccc']

```

#### 排序

- sort 
- reverse 逆序

```
a = [1,3,4,2]
a.reverse()
# [2,4,3,1]

a.sort()  # 默认正序
# [1,2,3,4]
a.sort(reverse=True)
#[4,3,2,1]
```

### 元组

- 元组被称为只读列表，即数据可以被查询，但不能被修改，所以，字符串的切片操作同样适用于元组。

    例：（1，2，3）（"a","b","c"）
- 儿子不能改，孙子可能可以改###


> 元组和列表不同之处在于

- 元组不能修改
- 元组使用小括号，列表使用方括号

```
a = ('a',1,2,3)
```

> 元组的不一致性

```
a = (1.0,2.0,3.0)
type(a)
#tuple

b = (1.0)
type(b)
# float
# 元组初始化的时候如果只有一个元素是不会初始化为元组的 会把括号里的值当作实际的类型处理

# 正确做法

c = (1.0,)
type(c)
# tuple
```

> 元组之修改孙子

```
a = ('a','b',[1,2,3],4)

a[2][0] = 'xxx'
# ('a','b',['xxx',2,3],4)
```

### 扩展内容

> #### join

```
s = 'abcde'
s1 = '+'.join(s) # a+b+c+d+e
```

> 再看

```
li = [1,2,3,4,5,6]
''.join(li) # 123456
```

> #### range

```
range() #就是一个元祖
for i in range(3,10):
    print(i) #循环打印一次，3-9的数字。
for i in range(10):
    print(i) #循环打印一次，0-9的数字。
for i in range(0,10,3):
    print(i) #循环打印一次，0-9的数字，步长3，间接输出0-9所有的被3整除的数。
for i in range(10,0,-2):
    print(i) #循环打印一次，10-1的数字，反向步长为2。
for i in range(10,-1,-2):
    print(i) #循环打印一次，10-0的数字，反向步长为2。
```

> 恶心的面试题

执行结果是啥？

```
for i in range(0,10,-1):
    print(i)
```

什么也不会执行。。。也不会报错。