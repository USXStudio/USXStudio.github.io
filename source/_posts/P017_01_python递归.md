---
title: P017_01_python递归
date: 2018-07-28 12:51:57
tags: fullstack
---

### 递归函数

什么是递归：在函数中调用自身函数

```
# 死循环
while 1:
    print(0)

# 如果用函数呢？
def aa():
    print('从前有座山')
    aa()

aa() #执行一部分后会报错
RecursionError: maximum recursion depth exceeded while calling a Python object
# 超过递归最大深度--因为一直的开辟空间函数没有结束 
```

测试递归深度

```
n = 0
def xx():
    global n
    n+=1
    print(n  )
    xx()

xx()

# 打印到998
```

> 递归的默认深度是997/998是python从内存角度出发做的限制

递归的深度是可以修改的，但是即使设置的过大依然要由硬件决定(我的hp能达到3222)

```
import sys
sys.setrecursionlimit(10000000)
n = 0
def xx():
    global n
    n+=1
    print(n  )
    xx()

xx()
```

> #### 如果你说需要1000次递归才能实现

我建议不要用递归，因为要1000次才能实现的需求说明递归不合适

#### 递归的缺点

- 如果递归次数过多，就不适合用递归
- 递归的缺点：占内存
- 递归的优点：会让代码变简单

#### 递归调用的定义

> 函数嵌套调用的一种特殊形式，函数在调用时，直接或间接调用了自身，就是递归调用

#### 递归分为两个阶段：

- 递推
- 回溯

> 经典问题==>阶乘

```
def fn(n):
    if (n == 1):
        return 1;
    else:
        return fn(n-1) * n;


a = fn(5)
print(a) # 120

```




