---
title: Py001_02_04Python再看字符串
date: 2018-08-13 22:54:30
tags: M01
---

### 字符串

有序的字符集合

> 特性

- 有序
- 不可变(一旦声明不可修改)

```
a = 'alex'
id(a) # 1435111012

a = 'bbb'
id(a) 3 3254554354 
内存地址变了

a修改后并不是在原地址上修改，而是新开辟一片内存空间。
那之前的alex去哪里了？ 答案是python解释器会定时的帮你把无用的地址清空掉
```

- swapcase()大小写翻转
- capitalize() 返回首字母大写其他全部小写的字符串
- casefold() 去掉大小写 全变成小写
- center() 返回字符填充 
- count() 统计字符个数
- endswith() 是否以什么结尾
- expandtabs() 设置制表符\t 的空格长度
- find() 查找字符串 从左往右返回索引，未找到返回-1
- format() 
- index() 返回查找字符串的索引，没有报错
- isalnum() 是否是阿拉伯的数字和字符(同时包含字母和数字)
- isdecimal() 是否是整数
- isidentifier() 是否是合法的变量(包含字母数字下划线，但不能是数字开头)
- islower() 是否是小写
- isnumeric() 是否有数字在里面
- isprintable() 这个值是否可以打印
- istitle() 是否是标题，每个单词首字母大写
- isupper()
- join() 列表拼接
- ljust() 字符串填充 s.ljust(50,'-')
- strip() 去除左右两边空格和换行
- lstrip()
- rstrip()
- maketrans()/translate() 通过字符生成某种映射关系，类似加密解密
- partition() 以输入字符将字符分成两段
- rpartition() 
- replace()
- rfind()
- rindex()
- split() 按指定字符来分
- splitlines() 按行来分
- startstwith()
- endswith()
- zfill() 

> format()

```
s = 'my name is {0}, i am {1} years old'
s.format('alex',22)

s2 = 'my name is {name}, i am {age} years old'
s.format(name='alex',age=22)
```

> index()

```
s = 'hello world'
s.index('o') # 4

s.index('o',5,6) # 报错
```

> 如果你是pycharm在这些函数上按住ctrl可以查看源码(但是只有解释没有实际的代码因为是c语言实现的)