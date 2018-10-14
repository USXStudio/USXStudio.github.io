---
title: Py001_02_05Python哈希和字典
date: 2018-08-14 11:08:43
tags: M01
---

### hash

hash值计算过程中依据这个值一些特征计算的，这就要求hash值必须固定，因此被hash的值必须是不可变的

> 不可变数据类型

- 数字
- 字符串
- 元组

> 可变数据类型

- 列表

> #### hash函数

```
hash('aaa') 
hash(1)
hash((1,2,3))
```

### 字典

键值对 k/v

> 特性

- k/v
- key必须放可hash ，且必须为不可变数据类型，必须唯一
- value可存放任意多个值，可修改，可以不唯一
- 无序
- 查找速度快

```
infos = {
    "name":'alex',
    "age":22
}
```

> crud

> 增加

```
info = {}
info['a'] = 2
```

> 修改

```
info = {"a":2}
info['a'] = 3
```

> 查找

```
info = {
    "a":1,
    "b":2,
    "c":3
}

"aa" in info  # 判断key在不在info里
info.get('a') # 1
info.get('aaa') # None

如果直接 info['aaa'] 会报错
```

> 删除

- pop(删除的key)
- popitem() 随机删
- del  info[key]删除某个key
- clear() 清空字典

> 其他方法

- keys() 获取key的列表
- values() 获取值的列表
- items() 将key和value放入到元组里，返回一个列表
- setdefault() 查看字典是否有这个key没有则设置默认值，有没有都返回这个key对应的值
- fromkeys() 批量生成字典，并设置默认值

### 集合类型

如何找出既买了iphone7和8的人

```
i7 = ["a","b","c","d"]
i8 = ["a","b","d"]
```

> 什么是集合

无序的，不重复的数据集合

- 去重，把一个列表变成集合就自动去重了
- 关系测试

```
s = {}
type(s) # dict 集合是空的时候被认为是字典

s = {1}
type(s) # set

s = set([1,2,2,3,4]) # {1,2,3,4}

s = {1,2,3,4,5,6,2,3,4} #  {1,2,3,4,5,6}
```

####  CRUD

```
s = {1,2,3,4,5}
s.add(2) # {1,2,3,4,5}
s.add(6) # {1,2,3,4,5,6}


s.discard(6) # 删除指定元素 {1,2,3,4,5}

s.discard(6) # 如果元素不存在不报错 {1,2,3,4,5}


s.remove(5) # {1,2,3,4}
s.remove(5) # 不存在报错

s.pop() # 随机删


s2 = {1,2,3}
s2.update([3,4,5,6])
# {1,2,3,4,5,6}
```

#### 集合的关系测试

```
i7 = {"a","b","c","d"}
i8 = {"a","b","e","f"}
```

> 交集 

```
第一种
i7.intersection(i8) # {"a","b"}

第二种
i7 & i8 # {"a","b"}
```

> 差集

```
# 买了i7没买i8的人
i7.difference(i8) # {"c","d"}
i7-i8


# 买了i8没买i7的人
i8.difference(i7) # {"e","f"}
i8-i7
```

> 并集

```
i7.union(i8) # {'d', 'e', 'f', 'c', 'a', 'b'}

i7|i8 
```

> 对称差集

并集取反

```
i7.symmetric_difference(i8) # {'f', 'e', 'c', 'd'}

i7 ^ i8
```

- difference_update 将差集的结果赋值给集合

> 关系

- isdisjoint 判断两个集合是不是不相交
- issuperset 是否包含
- issubset 是否被其他集合包含

```
a = {1,2,3,4}
b = {1,2,3}
print(b.issubset(a)) # False
print(b.issubset(a)) # True

print(b.issuperset(a)) # False
print(a.issuperset(b)) # True

```