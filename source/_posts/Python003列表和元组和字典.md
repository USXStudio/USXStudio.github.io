---
title: Python003列表和元组和字典
date: 2018-06-16 14:53:06
tags: python
---

### 列表

```
names =['aa','bb','cc']
type(names) 
# list
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

- extend

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

- del 根据索引删
- pop 删最后一个
- remove 根据值删

```
a = [1,2,3,4,5,6]
del a[0]
# [2,3,4,5,6]

a.pop()
# [2,3,4,5]
```

#### 排序

- sort 
- reverse 逆序

```
a = [1,3,4,2]
a.reverse()
# [2,4,3,1]

a.sort()
# [1,2,3,4]
a.sort(reverse=True)
#[4,3,2,1]
```

### 元组(tuple)

元组和列表不同之处在于

- 元组不能修改
- 元组使用小括号，列表使用方括号

```
a = ('a',1,2,3)
```

元组的不一致性

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

### 字典

- 键值对 k/v
- k不可重复

```
{key:value}

info = {
    "name":"悟空",
    "age":5000,
    "gender":"男"
}
```

#### 字典操作

```
info = {
    "name":"悟空",
    "age":5000,
    "gender":"男"
}
```

- 查

```
info['name'] 
info.get("name")
# "悟空"
```
- 修改
```
info["age"] = 10000
```

- 删除

```
# 删除 info里的 name字段
del info["name"]

# 整个删除
del info 
```

- 滞空

```
a = {a:1,b:2}
a.clear()
# {}
```

####  字典信息

```
info = {a:1,b:2,c:3}

len(info)
# 3

info.keys()
# ['a','b','c']

info.values()
# [1,2,3]
```

##### items

返回包含(k,v)元组的列表

```
info = {"a":1,"b":2}
info.items()
# [("a",1),("b":2)]
```
##### has_key

查看字典中key是否存在

```
info = {a:1,b:2}
info.has_key('a')
# True
```

##### 字典的遍历

> info = {a:1,b:2,c:3}

1. 遍历字典的key

```
for key in info.keys():
    print(key)
```

2. 遍历字典的value

```
for val in info.values():
    print(val)
```

3. 遍历字典的

```
for item in info.items():
    print(item)

# ("a",1) 
# ("b",2) 
# ("c",3) 

for key,val in info.items():
    print(key)
    print(val)
```

### 扩展问题

> 如何实现列表带索引的遍历

```
方法1 比较low的方法是声明个变量然后随着循环递增 很low 不写了

方法2 
a = ['a','b','c']
for i,v in enumerate(a):
    print(i)
    print(v)
```

> 「+ *」

```
"abc"+"123"
# "abc123"

[11,22,33]+[44,55,66]
# [11,22,33,44,55,66]



"-"*3
# "---"

[11,22,33]*2
# [11,22,33,11,22,33]

```

### 内置函数

- cmp 比较两个值
- len(item) 计算容器中元素个数
- max(item) 返回容器中最大的元素
- min(item) 返回容器中最小的元素
- del(item) 删除变量

```
cmp("hello","itcast")
# -1

cmp("itcast","hello")
# 1

cmp("hello","hello")
# 0
```
