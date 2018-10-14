---
title: P010_01_python命名空间和作用域
date: 2018-07-23 11:25:58
tags: fullstack
---

### 函数的命名空间

> 命名空间一共分为三种：

- 内置命名空间 —— Python解释器
    1. 就是python解释器一启动就可以使用的名字存储在内置命名空间中
    2. 内置的名字在启动解释器的时候被加载进内存里
    print() 、len()、list、tuple等
- 全局命名空间 —— 我们写的代码但不是函数中的代码
    1. 是在程序从上到下被执行的过程中依次加载进内存的
    2. 放置了我们设置的所有变量名和函数名
- 局部命名空间 —— 函数
    1. 就是函数内部定义的名字
    2. 当调用函数的时候，才会产生这个名称空间，随着函数执行的结束，这个命名空间又消失了

> 注意：

- 在局部:可以使用全局、内置命名空间中的名字
- 在全局:可以使用内置命名空间中的名字，但是不能用局部的
- 在内置:不能使用局部和全局的名字的

#### 命名空间的使用

> 全局不能访问局部

```
def func():
    a = 1

func()
print(a) # 报错 a为 func函数内部定义的变量为局部变量，不能在全局中访问
```

> 内置和全局都存在的max方法

这个非常神似js的作用域链

1. max为系统定义过的
2. 你在全局里重新定义max
3. 调用的时候就会使用全局里定义的(就近原则)

```
def max(l):
    print('in max func')

print(max([1,2,3]))
```

> python的关怀


- func  --> 函数的内存地址
- 函数名() 函数的调用
- 函数的内存地址() 函数的调用

```
def xxx():
    print(1)

print(xxx) # 打印出一个地址  <function xxx at 0x00000000004D2E18>
```

通过函数名调用明显比 调用形如「0x00000000004D2E18 」的十六进制数表示的内存地址要快。

(在一个班级的环境中：你是选择用人名来产生会话？还是身份证号。)

> xxx函数的调用过程

1. xxx() 的时候
2. 会现在全局范围内找有没有 xxx的定义，有则调用，反之去它的上一层也就是 内置命名空间，有则调用，反之报错

### 作用域

#### 作用域的种类

- 全局作用域 —— 作用在全局 —— 内置和全局名字空间中的名字都属于全局作用域  ——globals() 
    注：永远打印全局的名字
- 局部作用域 —— 作用在局部 —— 函数（局部名字空间中的名字属于局部作用域） ——locals()
    注：输出什么，根据locals所在的位置(根据调用位置来输出结果)

```
print(globals())
print(locals())
# 结果一样  因为都在全局作用域里
```

```
def func():
    a = 12
    b = 20
    print(locals())
    print(globals())

func()
# 结果不一样 
# globals在哪里调用都是输出全局作用域里的内容
# locals 这里只有   {'a': 12 , 'b': 20}
```

#### global关键字的作用

1. 对于不可变数据类型，在局部可查看全局作用域中的变量，但是不能直接修改
2. 如果想要修改的时候影响到全局，需要在程序的一开始添加global声明
3. 如果在一个局部（函数）内声明了一个global变量，那么这个变量在局部的所有操作将对全局的变量有效

> 不修改的情况下可以直接访问全局作用域的变量

```
a = 1
def xxx():
    print(a)

xxx() # 1
```

> 修改全局变量

```
a = 1
def xxx():
    a = 2
    print(a)

xxx() # 2
print(a) # 1

# 影响到全局
b = 1
def xxx():
    global b
    b = 2
    print(b)

xxx() # 2
print(b) # 2
```

### 函数的嵌套和作用域链

#### 1.函数的嵌套调用

```
def max(a,b):
    return a if a>b else b

def the_max(x,y,z):  #函数的嵌套调用
    c = max(x,y)
    return max(c,z)

print(the_max(1,2,3))
```

#### 2.函数的嵌套定义

```
def f1():
    print("f1")
    def f2():
        print("f2")
    f2()

f1()
```

#### 3.函数的作用域链

> 初学让你疑惑的执行结果！！！ 尤其是你会js 

```
def f1():
    a = 1
    def f2():
        a = 2
    f2()
    print('a in f1 : ',a)

f1() # a in f1 :  1

'''
f2中修改了 局部变量 a 的值
但是 在 f1中a值仍然为 1 

ps ： js里这个肯定是 2 
'''
```

> 原来python里要加 nonlocal 声明

```
def f1():
    a = 1
    def f2():
        nonlocal a
        a = 2
    f2()
    print('a in f1 : ',a)

f1() # a in f1 :  2
```

> #### nonlocal

首先，要明确 nonlocal 关键字是定义在闭包里面的。请看以下代码：

```
x = 0
def outer():
    x = 1
    def inner():
        x = 2
        print("inner:", x)

    inner()
    print("outer:", x)
 
outer()
print("global:", x)

结果：
inner: 2
outer: 1
global: 0
```

现在，在闭包里面加入nonlocal关键字进行声明：

```
x = 0
def outer():
    x = 1
    def inner():
        nonlocal x
        x = 2
        print("inner:", x)

    inner()
    print("outer:", x)
 
outer()
print("global:", x)

结果：
inner: 2
outer: 2
global: 0
```

> #### 区别：一个函数里面嵌套了一个函数。当使用 nonlocal 时，就声明了该变量不只在嵌套函数inner（）里面才有效， 而是在整个大函数里面都有效。

我们在看 global

```
x = 0
def outer():
    x = 1
    def inner():
        global x
        x = 2
        print("inner:", x)

    inner()
    print("outer:", x)
 
outer()
print("global:", x)

结果：
inner: 2
outer: 1
global: 2
```

结论：global 是对整个环境下的变量起作用，而不是对函数类的变量起作用。


#### 4.函数的本质

1. 可以被引用

    ```
    def func():
        print('in func')

    f = func
    print(f)
    ```
2. 可以被当作容器类型的元素

    ```
    def f1():
        print('f1')

    def f2():
        print('f2')

    def f3():
        print('f3')

    l = [f1,f2,f3]
    d = {'f1':f1,'f2':f2,'f3':f3}
    #调用
    l[0]()
    d['f2']()
    ```

3. 可以当作的参数和返回值

    ```
    def func():
        print(123)

    def wahaha(f):
        f()
        return f           #函数名可以作为函数的返回值

    qqxing = wahaha(func)   # 函数名可以作为函数的参数
    qqxing()
    ```

#### 5.闭包

> 嵌套函数，内部函数调用外部函数的变量

1. 常用方式

    ```
    def func():
        name = 'eva'
        def inner():
            print(name)
        return inner

    f = func()
    f()
    ```

2. 判断闭包函数的方法 closure

```
#输出的__closure__有cell元素 ：是闭包函数
def func():
    name = 'eva'
    def inner():
        print(name)
    print(inner.__closure__)
    return inner

f = func()
f()

结果：
(<cell at 0x0000000001ECA4C8: str object at 0x0000000001DCB1B8>,)
eva
-------------------------华丽的分割线--------------------------------------------------------------
#输出的__closure__为None ：不是闭包函数
name = 'egon'
def func2():
    def inner():
        print(name)
    print(inner.__closure__)
    return inner

f2 = func2()
f2()

结果：
None
egon
```

3. 闭包嵌套

```
def wrapper():
    money = 1000
    def func():
        name = 'eva'
        def inner():
            print(name,money)
        return inner
    return func

f = wrapper()
i = f()
i()

结果：
eva 1000
```

4. 闭包函数获取网络应用

```
from urllib.request import urlopen

def index():
    url = "http://www.xiaohua100.cn/index.html"
    def get():
        return urlopen(url).read()
    return get

xiaohua = index()
content = xiaohua()
print(content)
```
    
