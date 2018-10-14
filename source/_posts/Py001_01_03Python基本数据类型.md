---
title: Py001_01_03Python基本数据类型
date: 2018-08-13 20:10:06
tags: M01
---

> 与其他语言一样python也有一些数据类型，但是它不会像java那样强制你声明好数据的类型

python解释器会自己识别你的数据类型

```
//java
int age = 12
char x = 'a'

# python
age = 12
```
### 判断数据类型

- type函数输出变量的数据类型

```
a = 1
b = 'aa'

type(a) # int
type(b) # str
```

### 基本数据类型

- 数字
- 字符串
- 布尔

#### 数字类型

> int(整型)

```
在32位机器上，整数的位数为32，取值范围为-2**31至2**31-1
在64位机器上，整数的位数为64，取值范围为-2**63至2**63-1
```

> long(长整型)

```
跟C语言不同，python没有指定长整型的位宽，即没有限制长整型数值的大小，但实际上由于机器内存有限，我们使用的长整型也不可能无限大。
```

> 从python2.2起，如果整数溢出，python会自动将整数数据转换为长整数。所以现在在长整数后面加不加L也不会导致严重后果了

> #### 注意

- 在python3里不再有long类型了，全是int
- 除了int,long还有float浮点数，复数，但今天先不涉及

#### 字符串

加引号的就被认为是字符串

```
name = 'hjx'
name2 = "alex"
msg = '''My name is Alex,I am 22 years old '''
```

如果想使用单引号作为字符呢？

```
msg = "My name is Alex,I'am 22 years old"
```

> 多引号的作用：就是多行的时候必须使用它

```
msg = '''
一二三四五
上山打老虎
'''
```

> 字符串拼接

python不仅支持「+」 还支持 「*」

```
name = 'alex'
age = '22'
print(name+age) # alex22

print(name*3) # alexalexalex
```

#### 布尔类型

主要用来逻辑判断。

- True
- False

```
a = 12
b = 5
print( a > b ) # True
```

> 几个falsy值

- ''
- 0
- []
- ()
- {}
- None
- False

#### 格式化输出

```
name = input('name')
age = input('age')
job = input('job')
city = input('city')


info = '''
----------info of %s
Name:       %s
Age :       %s
Job :       %s
City:       %s
''' % (name,age,job,city)

print(info)
```

占位符

- %s 就是string
- %d 就是数字
- %f 就是float

修改Age的占位符为 %d

```
程序报错

因为input方法接收的任何输入都是字符串

你应该这样,将输入的结果转换为数字

age = int(input('age'))
```