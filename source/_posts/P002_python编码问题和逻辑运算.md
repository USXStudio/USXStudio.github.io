---
title: P002_python编码问题和逻辑运算
date: 2018-07-19 22:11:11
tags: fullstack
---

### 1.知识点回顾

```
#1、使用while循环输入 1 2 3 4 5 6 8 9 10
'''
count = 0
while count < 10:
    count += 1  # count = count + 1
    if count == 7:
        print(' ')
    else:
        print(count)

count = 0
while count < 10:
    count += 1  # count = count + 1
    if count == 7:
        continue
    print(count)
'''
#3、输出 1-100 内的所有奇数
#方法一：
# count = 1
# while count < 101:
#     print(count)
#     count += 2
#方法二：
# count = 1
# while count < 101:
#     if count % 2 == 1:
#         print(count)
#     count += 1

#5、求1-2+3-4+5 ... 99的所有数的和
# sum = 0
# count = 1
# while count < 100:
#     if count % 2 == 0:
#         sum = sum - count
#     else:
#         sum = sum + count
#     count += 1
# print(sum)

#6、用户登陆（三次机会重试）
#input 心中有账号，密码 while

i = 0
while i < 3:
    username = input('请输入账号：')
    password = int(input('请输入密码：'))
    if username == '咸鱼哥' and password == 123:
        print('登录成功')
    else:
        print('登录失败请重新登录')
    i += 1
```

### 2.格式化输出

```
#格式化输出
# % s d
# name = input('请输入姓名')
# age = input('请输入年龄')
# height = input('请输入身高')
# msg = "我叫%s，今年%s 身高 %s" %(name,age,height)
# print(msg)

"""
name = input('请输入姓名:')
age = input('请输入年龄:')
job = input('请输入工作:')
hobbie = input('你的爱好:')

msg = '''------------ info of %s -----------
Name  : %s
Age   : %d
job   : %s
Hobbie: %s
------------- end -----------------''' %(name,name,int(age),job,hobbie)
print(msg)
"""

name = input('请输入姓名')
age = input('请输入年龄')
height = input('请输入身高')
msg = "我叫%s，今年%s 身高 %s 学习进度为3%%s" %(name,age,height)
print(msg)
```

> 想格式化输出%就要 两个%%代表转义，否则报错

### 3. while else


> 与其它语言else 一般只与if 搭配不同，在Python 中还有个while ...else 语句

> while 后面的else 作用是指，当while 循环正常执行完，中间没有被break 中止的话，就会执行else后面的语句

```
count = 0
while count <= 5 :
    count += 1
    print("Loop",count)

else:
    print("循环正常执行完啦")
print("-----out of while loop ------")

输出如下：

Loop 1
Loop 2
Loop 3
Loop 4
Loop 5
Loop 6
循环正常执行完啦
-----out of while loop ------
```

如果执行过程中被break啦，就不会执行else的语句啦

```
count = 0
while count <= 5 :
    count += 1
    if count == 3:break
    print("Loop",count)

else:
    print("循环正常执行完啦")
print("-----out of while loop ------")

输出如下：

Loop 1
Loop 2
-----out of while loop ------
```

### 4.初始编码

- 最早的'密码本' ascii 涵盖了英文字母大小写，特殊字符，数字。
01010101
- ascii 只能表示256种可能，太少，
- 创办了万国码 unicode
    16表示一个字符不行，32位表示一个字符。
    A  01000001010000010100000101000001
    B  01000010010000100100001001000010
    我 01000010010000100100001001000010
- Unicode 升级 utf-8  utf-16 utf-32
    8位 = 1字节bytes
    1. utf-8 一个字符最少用8位去表示，英文用8位  一个字节
          欧洲文字用16位去表示                两个字节
          中文用24 位去表示                   三个字节
    2. utf-16 一个字符最少用16位去表示

- gbk 中国人自己发明的，一个中文用两个字节 16位去表示。

### 5. 逻辑运算

- and  (JS里的 &&) 
- or   (JS里的 ||)
- not  (JS里的 !)
```
#and or not
#优先级，（）> not > and > or
# print(2 > 1 and 1 < 4)
# print(2 > 1 and 1 < 4 or 2 < 3 and 9 > 6 or 2 < 4 and 3 < 2)
# T or T or F
#T or F
# print(3>4 or 4<3 and 1==1)  # F
# print(1 < 2 and 3 < 4 or 1>2)  # T
# print(2 > 1 and 3 < 4 or 4 > 5 and 2 < 1)  # T
# print(1 > 2 and 3 < 4 or 4 > 5 and 2 > 1 or 9 < 8)  # F
# print(1 > 1 and 3 < 4 or 4 > 5 and 2 > 1 and 9 > 8 or 7 < 6)  # F
# print(not 2 > 1 and 3 < 4 or 4 > 5 and 2 > 1 and 9 > 8 or 7 < 6) # F

#ps  int  ----> bool   非零转换成bool True   0 转换成bool 是False
# print(bool(2))
# print(bool(-2))
# print(bool(0))

# #bool --->int
# print(int(True))   # 1
# print(int(False))  # 0


'''x or y x True，则返回x'''
# print(1 or 2)  # 1
# print(3 or 2)  # 3
# print(0 or 2)  # 2
# print(0 or 100)  # 100


# print(2 or 100 or 3 or 4)  # 2

# print(0 or 4 and 3 or 2)
'''x and y x True，则返回y'''
# print(1 and 2)
# print(0 and 2)
print(2 or 1 < 3)
print(3 > 1 or 2 and 2)
```


