---
title: Python012列表推导式
date: 2018-06-18 15:29:49
tags: python
---

### 列表推到式

```
a = [x for x in [11,22,33,44,55]]
# [11,22,33,44,55]

# 生成1-100的列表
b = [x for x in range(1,101)]

# 生成1-100的奇数列表
b = [x for x in range(1,101,2)]


# 生成1-100的偶数列表
b = [x for x in range(2,101,2)]
# 生成1-100的偶数列表
c = [x+1 for x in range(1,101,2)]

# 生成1-100的偶数列表  只能写一个条件
d = [x for x in range(1,101) if x%2==0]
```

> 列表推到式，循环嵌套

```
e = [x for x in range(1,4) for y in range(1,3)]

# x是外层循环 循环3次
# y是内层循环 循环2次

# [1,1,2,2,3,3]      


# 想要既取到x也取到y 生成元组
e = [(x,y) for x in range(1,4) for y in range(1,3)] 
# [(1,1),(1,2),(2,1),(2,2),(3,1),(3,2)]
```