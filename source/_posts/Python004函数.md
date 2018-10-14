---
title: Python004函数
date: 2018-06-16 16:59:05
tags: python
---

### 函数

- 声明关键字 def

```
# 函数声明
def aa():
    print("1")
    print("2")
    print("3")
    print("4")
    print("5")

# 函数调用

aa()
```

> 带参数的函数

```
def sum(m,n):
    print(m+n)

sum(1,2)
# 3


# sum(1,2,3) 报错  参数要几个传几个
```

> 注意事项

- def顶格写
- 函数内要有缩进

> 函数返回值

```
def sum(a,b):
    return a+b
```

return的作用

- 把结果返回给调用者
- 结束一个函数
- 如果写了多个return程序不会出现语法错误，并把第一个return的值返回

#### 注意点函数声明要放在最前

> 它不像js一样函数声明后会提升

- python里函数声明要前置
- 先定义后使用

#### 局部变量和全局变量

- 在函数里定义的变量叫局部变量,离开函数就没了
- 形参也是局部变量

```
def test():
    a = 1
    print(a)

# 1
test() 

# 报错
print(a)
```



- 全局变量就是定义在函数外

```
num = 100

def test():
    #运行正常
    print(num)
    #报错 全局变量在函数里不能改，会报错
    num+=2

test()
# 报错
```

> 如何解决全局变量修改报错的问题

- 在函数内部加上一个 global 声明

```
num = 100

def test():
    global num
    print(num)
    num+=2

test()
# 102
```

#### 缺省参数

我的理解就是给参数设置默认值，你传递了就用你的不传递走默认(兜底)

```
def xxx(a,b,c):
    print(a)
    print(b)
    print(c)

# xxx(1,2) 报错 因为指定要三个参数你传少了

def yyy(a,b,c=33)
    print(a)
    print(b)
    print(c)

yyy(11,22)
# 11
# 22
# 33
```

- 缺省参数应该放在最后,避免歧义

```
# 非常歧义的写法 你传递4个参数正确 传递3个我不知道自己试！！！
def xxx(a,b,c=33,d):
    print(a)
    print(b)
    print(c)
    print(d)

#多个缺省参数

def yyy(a,b,c,d=44,e=55):
    print(a)
    print(b)
    print(c)
    print(d)
    print(e)

yyy(11,22,33,1000)
'''
11
22
33
1000
55
'''

# 乱序的传参数(前提你要知道形参叫什么)
def zzz(a,b):
    print(a)
    print(b)

zzz(b=2,a=1)


# 我就想给e传递参数

yyy(11,22,33,e=2000)
'''
11
22
33
44
2000
'''
```

#### 不定长参数

```
def xx(a,b,*args,**kwargs):
    print(a)
    print(b)
    print(args)
    print(kwargs)

xx(11,22,33,44,55,66,mm=11,nn=22)
'''
11
22
(33,44,55,66)
{mm:11,nn:22}
'''
```

> 规定

- *args 写一个「 * 」表示传递多个没有名字的值的时候放入 args
- **kwargs 写俩个「 ** 」表示传递多余的参数带有名字的值的时候放入 kwargs

```
A = [11,22,33]
B = {aa:100,bb:200}

xx(11,22,A,B)
'''
11
22
([11,22,33],{aa:100,bb:200})
{}
'''

xx(11,22,A,pp=B)
'''
11
22
([11,22,33])
{pp:{aa:100,bb:200}}
'''

# 没有名字的参数前加一个 *
xx(11,22,*A,B)
'''
11
22
(11,22,33,{aa:100,bb:200})
{}
'''

# 有名字的参数前加两个 **
xx(11,22,*A,**B)
'''
11
22
(11,22,33)
{aa:100,bb:200}
'''
```

> 当列表/元组在做实参传递的时候，如果前面有一个*，表示对其进行解包

```
[11,22,33] =======> 11,22,33
```

> 当字典在做实参传递的时候，如果前面有两个*，表示对其进行解包

```
{aa:100,bb:200} =======> aa=100,bb=200
```

### 引用

- 可变数据类型 列表/字典
- 不可变数据类型 数字/字符串/元组

字段的key可以为。。。

```
# 运行正常
M = {a:100,100:100,3.14:314}
M = {(11,22,33):10086}



# 报错
M = {[11,22]:1122} 

```

### 匿名函数 lambda表达式

```
def test(a):
    return a+1

# 匿名函数 如果是单独使用需要赋值给一个变量
aaa = lambda a:a+1

test(10)
aaa(10)

# 多个参数

bbb = lambda a,b:a+b
```

> 注意事项

- 匿名函数不能写print

把函数当作参数

```
def test(a,b,fn):
    return fn(a,b)

test(11,22,lambda x,y:x+y)
# 33
```

排序方法里使用

```
aa = [
    {index:22,age:18},
    {index:13,age:22},
    {index:33,age:12}
]

# key代表指定的排序列
aa.sort(key = lambda x:x['index'])
'''
[
    {index:13,age:22},
    {index:22,age:18},
    {index:33,age:12}
]

aa.sort(key = lambda x:x['index'],reverse=True)
'''
[
    {index:33,age:12},
    {index:22,age:18},
    {index:13,age:22}
]

'''
```