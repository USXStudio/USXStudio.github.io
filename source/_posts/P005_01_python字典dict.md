---
title: P005_01_python字典dict
date: 2018-07-22 11:15:06
tags: fullstack
---

### dict

> 神似js里的对象字面量 k/v组成复杂数据类型

但是有更加让人惊讶的方式

```
dic = {
    'name':['大乔','小乔'],
    'py3':[{'num':71,'age':18}],
    True:1,
    (1,2,3):'aaa',
    2:'周瑜'
}
```

- 增

没有则增加，有则覆盖

```
a = {'age':18,'gender':1}
a['name'] = 'xxx'
a['age'] = 22
```

> 增的扩展

有键值对，不做更改，没有才添加

```
a = {'age':18,'gender':1}
a.setdefault('weight') # a['weight'] 的值为None

a.setdefault('yy',2) # a['yy'] 的值为 2


如果设置默认值的时候在字典初始化的时候已经有值了 就会用其初始的值
a.setdefault('gender',2) # a['gender'] 的值为 1
```

- 查

```
info = {'age':15,name:'aa'}
info['name'] 
info.get("name")


----------------------------
注意 如果字典里没有 name字段
a['name']就会报错
a.get('name') 返回 None
# 返回没有键的情况下返回自定义的值
a.get('xxxx','没有这个键')
```
- 修改
```
info["age"] = 10000

a = {'name':'aa','age':18,}
b = {'name':'bb','weight':50}
# 将a的所有键值对 更新到b里 
b.update(a) 
# {'name':'aa','age':18',weight':50}
```

- 删除

```
# 删除 info里的 name字段
del info["name"]

# pop的删-->按照键去删
info.pop('name') # 如果info里没有name字段就会报错

# 如何删除你不知道存在不存在的键，同时不报错
# 设置返回值
info.pop('name',None) # 即使name不存在也不会报错

# 随机删除 返回值是元组，删除的键值
info.popitem()

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