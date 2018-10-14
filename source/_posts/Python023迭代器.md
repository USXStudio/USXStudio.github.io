---
title: Python023迭代器
date: 2018-06-22 12:58:16
tags: python
---

### 迭代器

#### 先来了解什么是可迭代对象

> 集合类型

- list
- tuple
- dict
- set
- str

> generator 生成器

#### 判断是否可以迭代

使用 isinstance()判断一个对象是否是 Iterable对象(可迭代)

```
from collections import Iterable

isinstance([],Iterable) # True
isinstance('abc',Iterable) # True
isinstance({},Iterable) # True
isinstance((x for x in range(10)),Iterable) # True
isinstance(100,Iterable) # False
```

#### 迭代器可以调用next()

Iterator可迭代对象，可迭代对象才可以使用next

- 生成器就是可迭代对象
- 列表不是可迭代对象但是它可以迭代

```
from collections import Iterator

isinstance((x for x in range(10)),Iterator) # True


isinstance([],Iterator) # False
isinstance('abc',Iterator) # False
isinstance({},Iterator) # False
isinstance(100,Iterator) # False
```

#### iter()函数

可以把「非可迭代对象」转换为「可迭代对象」

```
isinstance(iter([]),Iterator) # True

isinstance(iter('abc'),Iterator) # True

# 生成器比实际的列表占用空间小
```