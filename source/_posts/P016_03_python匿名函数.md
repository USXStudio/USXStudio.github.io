---
title: P016_03_python匿名函数
date: 2018-07-28 11:55:36
tags: fullstack
---

### 匿名函数

为实现很简单需求而设计的一句话函数

#### 匿名函数使用规范

- 匿名函数关键字是lambda
- lambda写在最前面其后跟参数，然后以「:」为分割,最后实际的逻辑也就是最后的结果
- 必须一行结束
- 多个参数用「,」分开

> 万能公式  lambda 参数 : 返回值

```
xxx = lambda n:n*n

#两个参数
xxx = lambda m,n:m*n
```

> 求字段里值最大的

```
xx = {'k1':10,'k2':20,'k3':30}

'''
def fn(key):
    return xx[key]
'''
max(xx,key=lambda k:xx[k])
```

#### 容易拿分的面试题

> 1.问执行结果

```
d = lambda p:p*2
t = lambda p:p*3

x = 2
x = d(x) # 4
x = t(x) # 12
x = d(x) # 24
print(x)
```

> 2.有俩元组(('a'),('b')),(('c'),('d'))如何转换为列表[{'a':'c'},{'b':'d'}]

提示zip拉链方法

```
# step001 先用zip把格式转换
a = (('a'),('b'))
b = (('c'),('d'))

res = zip(a,b) # 此时是一个生成器

'''
for i in res:
    print(i)

结果：
('a','c') 
('b':'d')
'''

# step002 遍历这个生成器 将元组变为字典
# filter/map/sorted/max/min

# 分析出：明显是用map
def func(tup):
    return {tup[0]:tup[1]}
res = map(func,res)

# step003 优化用lambda

res = map(lambda tup:{tup[0]:tup[1]},res)
print(list(res))

# step004 各种优化
res = map(lambda tup:{tup[0]:tup[1]},zip(a,b))

res = list(map(lambda tup:{tup[0]:tup[1]},zip(a,b)))

```

> 3.刁钻面试题

```
def xx():
    return (lambda x:i*x for i in range(4))

[m(2) for m in xx()]

结果
[0,2,4,6]

解析
先看xx函数
def xx():
    return (lambda x:0*x,lambda x:1*x,lambda x:2*x,lambda x:3*x)
[m(2) for m in xx()]

m(2)实际就是个函数 并传递参数2带入。。。
[0,2,4,6]
```

#### 推荐一个流程图的工具ProcessOn
