---
title: Python002字符串
date: 2018-06-16 14:11:27
tags: python
---

### 字符串查找

> 字符串内查找字符串返回索引

- find 从左往右，找不到返回 -1
- rfind 从右往左，找不到返回 -1
- index 从左往右，找不到报错
- rindex 从右往左，找不到报错

```
str = 'aa bbb cc dd ee'

str.find('aa') 1
str.find('ff') -1
... 懒得写了自己试吧！！！
```

> 返回字符串在字符串内出现的次数

```
str = 'aa bb cc dd aa'
str.count('aa')  #2
```

### 字符串替换replace

```
str = 'python'
str.replace('py','Py')
```

### split

```
str = 'hello world ha ha'

str.split(" ")  
# ['hello','world','ha','ha']

str.split(" ",2)  
# ['hello','world','ha ha']
```

### capitalize

把字符串第一个字母变大写

```
a = 'aa bb cc'
a.capitalize()
Aa bb cc
```
### swapcase

```
a = 'aa bb cc'
print(a.swapcase()) #大小写翻转
# AA BB CC
```

### title 

把字符串每个单词首字母大写

```
a = 'aa bb cc'
a.title()
Aa Bb Cc
```

### 大小写

- startswith 
- endswith

判断字符串是否以字符串 开头/结尾 

```
a = 'Hello'

a.startswith('He')
# True

b = 'World'
a.endswith('aa')
# False
```

- lower 全部转小写
- upper 全部转大写

### 字符串各种处理

- ljust 指定一个字符长度，返回原字符串左对齐，并用空格填充剩余部分
- rjust 指定一个字符长度，返回原字符串右对齐，并用空格填充剩余部分
- center 指定一个字符长度，返回原字符居中对齐，并用空格填充剩余部分

```
a = 'hello'
a.ljust(10)
# 'hello     '

a = 'hello'
a.rjust(10)
# '     hello'

a = 'hello'
a.center(30)
# '          hello          '
```

- lstrip 删除字符串左面空白
- rstrip 删除字符串右面空白
- strip 删除字符串空白

```
a = '   hello'
a.lstrip()
# 'hello'
```


- partition 把字符分成三部分从左开始
- rpartition 把字符分成三部分从右开始

```
a = 'What are you doing'
a.partition('are')
# ['What','are','you doing']
```

- splitlines 按照换行符分割

```
a = 'hello\nworld'
a.splitlines()
# ['hello','world']
```

- isalpha 如果字符串所有字符都是字母返回True
- isdigit 如果字符串所有字符都是数字返回True
- isalnum 如果字符串所有字符都是字母或数字返回True
- isspace 如果字符串所有字符都是空格返回True

### 数组拼接

join

```
str = '_'
a = ['Hi','boy']
str.join(a)
# 'Hi_boy'
```

### format的三种玩法 格式化输出

```
res='{} {} {}'.format('egon',18,'male')
res='{1} {0} {1}'.format('egon',18,'male')
res='{name} {age} {sex}'.format(sex='male',name='egon',age=18)
```

### strip

```
name='*egon**'
print(name.strip('*'))
print(name.lstrip('*'))
print(name.rstrip('*'))

'''
egon
egon**
*egon
'''
```