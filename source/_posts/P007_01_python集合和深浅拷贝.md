---
title: P007_01_python集合和深浅拷贝
date: 2018-07-22 12:21:41
tags: fullstack
---

### 集合

> 1.集合的创建

```
set1 = set({1,2,'barry'})
set2 = {1,2,'barry'}
print(set1,type(set1))
print(set2,type(set2))
结果：
{1, 2, 'barry'} <class 'set'>
{1, 2, 'barry'} <class 'set'>
```

> 2.集合的增

- 直接增加

```
set1 = {'alex','wusir','ritian','egon','barry'}
set1.add('女神')
print(set1)  #增加，无序的。
结果：
{'ritian', 'alex', 'egon', '女神', 'wusir', 'barry'}
```
- 迭代增加

```
set1 = {'alex','wusir','ritian','egon','barry'}
set1.update('A')
print(set1)
set1.update('老师')
print(set1)
set1.update([1,2,3])
print(set1)
结果：
{'egon', 'alex', 'ritian', 'wusir', 'A', 'barry'}
{'egon', '老', 'alex', 'ritian', 'wusir', 'A', '师', 'barry'}
{'egon', '老', 1, 2, 3, 'alex', 'ritian', 'wusir', 'A', '师', 'barry'}
```

> 3.集合的删

- 3.1 .remove('元素')


```　
set1 = {'alex','wusir','ritian','egon','barry'}
set1.remove('alex') #删除一个元素
print(set1)
结果：{'wusir', 'ritian', 'barry', 'egon'}
```

- 3.2 .pop()

```
set1 = {'alex','wusir','ritian','egon','barry'}
set1.pop() #随机删除一个元素
print(set1)
结果：
{'alex', 'ritian', 'wusir', 'egon'}
```

- 3.3 .clear()
```
set1 = {'alex','wusir','ritian','egon','barry'}
set1.clear() #清空集合
print(set1)
结果：
set()
```
- 3.4 del
```
set1 = {'alex','wusir','ritian','egon','barry'}
del set1 #删除集合，会报错
print(set1)
结果：报错
```

> 4.集合之大招

- 4.1交集 （& 或者 intersection）

```
set1 = {1,2,3,4,5}
set2 = {4,5,6,7,8}
print(set1 & set2)
print(set1.intersection(set2))
结果：
{4, 5}
{4, 5}
```

- 4.2并集 （| 或者 union）

```
set1 = {1,2,3,4,5}
set2 = {4,5,6,7,8}
print(set1 | set2)
print(set1.union(set2))
结果：{1, 2, 3, 4, 5, 6, 7, 8}
{1, 2, 3, 4, 5, 6, 7, 8}
```

- 4.3差集 （- 或difference）
```
set1 = {1,2,3,4,5}
set2 = {4,5,6,7,8}
print(set{1, 2, 3}1 - set2)
print(set1.difference(set2))
结果：
{1, 2, 3}
{1, 2, 3}
```

- 4.4反交集（^ 或者 symmetric_difference）
```
set1 = {1,2,3,4,5}
set2 = {4,5,6,7,8}
print(set1 ^ set2)
print(set1.symmetric_difference(set2))
结果：
{1, 2, 3, 6, 7, 8}
{1, 2, 3, 6, 7, 8}
```

- 4.5子集与超集(< 或者 issubset)
```
set1 = {1,2,3}
set2 = {1,2,3,4,5,6}
print(set1 < set2)
print(set1.issubset(set2))
结果：
True
True

print(set2 > set1)
print(set2.issuperset(set1))
结果：
True
True
```

> 5.frozenset不可变集合，让集合变成不可变类型。

```
s = frozenset('barry')
print(s,type(s))  
# frozenset({'a', 'y', 'b', 'r'}) <class 'frozenset'>
```

### 深浅拷贝

请参考我之前写的知识点总结《Python021==和 is和深浅拷贝》

> 深浅copy的区别
1. 对于浅copy来说，第一层创建的是新的内存地址，而从第二层开始，指向的都是同一个内存地址，所以，对于第二层以及更深的层数来说，保持一致性。

2. 对于深copy来说，两个是完全独立的，改变任意一个的任何元素（无论多少层），另一个绝对不改变。
