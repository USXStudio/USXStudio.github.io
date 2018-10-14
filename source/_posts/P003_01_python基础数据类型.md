---
title: P003_01_python基础数据类型
date: 2018-07-19 22:50:33
tags: fullstack
---

### 1. 数字int。

> 数字主要是用于计算用的，使用方法并不是很多，就记住一种就可以：

```
#bit_length() 当十进制用二进制表示时，最少使用的位数
v = 11
data = v.bit_length()
print(data)

'''
0000 0000   0  1
0000 0001   1  1
0000 0010   2  2
0000 0011   3  2
0000 0100   4  3
0000 0101   5  3
'''
```

### 2.布尔值bool

> 布尔值就两种：True，False。就是反应条件的正确与否。

- 真   1   True。
- 假   0   False。

> 与js的不同点 后四个也是False,但是在js里 只要是对象就是true

- 0 
- ''
- []
- ()
- {}
- set()


> int ----> str
```
i = 1
s = str(i)
```

> str ---> int
```
s = '123'
i = int(s)

s2 = '123a'
i2 = int(s2) #报错
```

> int ----->bool  只要是0 ----》False  非0就是True
```
i = 3
b = bool(i)
print(b)
```
> bool----> int

```
#True   1
#False  0
'''
```
> ### ps:大神的高端写法

```
while True:
    pass
while 1:  # 效率高
    pass
```

> str --->bool
```
#s = "" -----> False
#非空字符串都是True
#s = "0" -----> True
# s
# if s:
#     print('你输入的为空，请重新输入')
# else:
#     pass
```

### 3.字符串str

#### 字符串的索引与切片。

> 索引即下标，就是字符串组成的元素从第一个开始，初始索引为0以此类推。

```
a = 'ABCDEFGHIJK'
print(a[0])
print(a[3])
print(a[5])
print(a[7])

#超过字符串长度咋办？
a[0:70] 'ABCDEFGHIJK'
```

切片就是通过索引（索引：索引：步长）截取字符串的一段，形成新的字符串（原则就是包头不包尾）。

```
a = 'ABCDEFGHIJK'
print(a[0:3]) # ABC
print(a[2:5]) # CDE
print(a[0:])  # 默认到最后 ABCDEFGHIJK
print(a[0:-1]) # -1就是最后一个 ABCDEFGHIJ
print(a[0:5:2]) #加步长 ACE
print(a[5:0:-2]) #反向加步长 FDB
```

> 索引

```
s = 'ABCDLSESRF'
s1 = s[0] # A
s2 = s[2] # C
s3 = s[-1] # F
s4 = s[-2] # R
```
> ABCD   切片 ：包头不包尾

```
s = 'ABCDLSESRF'
s5 = s[0:4]  # ABCD
s6 = s[0:-1] # ABCDLSESR

s7 = s[:] # ABCDLSESRF
s8 = s[0:] #ABCDLSESRF

s9 = s[0:0] # ''空字符


s = 'ABCDLSESRF'    # s[首:尾:步长]
s10 = s[0:5:2]  # ACL
s11 = s[4:0:-1] # LDCB
s12 = s[3::-1]  # DCBA
s13 = s[3::-2]  # DB
s14 = s[-1::-1] # FRSESLDCBA
s15 = s[::-1]   # FRSESLDCBA
```

> #### 简洁写法 都可以获取整个字符串
```
name[0:]
name[:]
name[::]

# name[]报错
```
#### 字符串常用方法

> 也可以参考我之前写的python文章 《Python002字符串》

![](https://raw.githubusercontent.com/slTrust/note/master/python/p003_01.jpg)


```
#字符串的操作
s = 'alexWUsir'
s1 = s.capitalize()  # 首字母大写

# 全大写，全小写
s2 = s.upper()
s21 = s.lower()

#大小写翻转
# s3 = s.swapcase()

# 每个隔开(特殊字符或者数字)的单词首字母大写
# s = 'alex*egon-wusir'
# s4 = s.title()

# s = 'fade,crazy*w4rri0r_songsong node_3'
# s4 = s.title()

#居中，空白填充
# s = 'alexWUsir'
# s5 = s.center(20,'~')

# s = 'alex\tsir'
# s6 = s.expandtabs()

# s = 'alex二哥'
# #公共方法
# l = len(s)

#以什么开头结尾 endswith
# s = 'alexWUsir'
# s7 =s.startswith('alex')
# s71 = s.startswith('e',2,5)

# find 通过元素找索引，找不到返回-1
#
# index通过元素找索引，找不到报错
# s = 'alexWUsir'
# s8 = s.find('A')
# s81 = s.index('A')

#strip rstrip lstrip
# s = 'alexWUsir%'
# s9 = s.strip('%')

# s = ' *a%lexWUsi* r%'
# s91 = s.strip(' %*')

# strip 默认删除前后空格

# username = input('请输入名字：').strip()
# if username =='春哥':
#     print('恭喜春哥发财')

# s = 'alexaa wusirl'
# s10 = s.count('al')
# print(s10)

# split   str ---->list
# s = ';alex;wusir;taibai'
# l = s.split('a')
# print(l)

# s = '来看待街坊邻居复合大师街坊法好的撒见客户'
# s11 = s.replace('街坊','老王',1)

# s = 'fhdsklfds'
# for i in s:
#     print(i)
s = 'fdsa苍井空fdsalk'
if '苍井空' in s:
    print('您的评论有敏感词...')
```

#### format的格式化输出

三种玩法 

```
res='{} {} {}'.format('egon',18,'male')
res='{1} {0} {1}'.format('egon',18,'male')
res='{name} {age} {sex}'.format(sex='male',name='egon',age=18)
```

#### 循环字符串

```
a = 'abcdefg'
for temp in a:
    print(temp) 


i = 0 ;
length = len(a)
while i<length:
    print(a[i])
    i+=1
```

![](https://raw.githubusercontent.com/slTrust/note/master/python/p003_02.jpg)


