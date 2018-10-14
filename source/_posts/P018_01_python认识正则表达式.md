---
title: P018_01_python认识正则表达式
date: 2018-07-28 17:19:18
tags: fullstack
---

### 正则表达式

> 正则是通用的(几乎所有编程语言都适用的一个规则)

python里想使用正则只能引入re模块

```
import re
```

一个推荐的工具网站

[正则工具网站](http://tool.chinaz.com/regex)

具体要记住的规则请参考我之前写的
[难以启齿的正则]文章

知识点总结的博客

[Eva_J](http://www.cnblogs.com/Eva-J/articles/7228075.html)

> 匹配手机号的例子

```
import  re
phone_num = input('请输入手机号')
if re.match('^(13|14|15|18)[0-9]{9}$',phone_num):
    print('合法')
else:
    print('非法')
```

> 字符串中r的应用

原样匹配(取消「\」的转义)

```
正则 \\n 匹配 换行\n
```

> 贪婪匹配

```
<.*>  正则

<script>....</script>
# 匹配结果
<script>....</script>
```

> 惰性匹配

```
<.*?>  正则
<script>....</script>
# 匹配结果
<script>
</script>
```

#### 几个常用的贪婪匹配

- *?重复任意次，但尽可能少的重复
- +?重复1次或更多次，但尽可能少的重复
- ??重复0次或1次，但尽可能少的重复
- {n,m}?重复n-m次，但尽可能少的重复
- {n,}?重复n次以上，但尽可能少的重复

#### .*?的用法

```
. 任意字符
* 取0至无限长度
? 非贪婪模式


实例： .*?x
取前面任意的长度但是碰见x就结束了
```

### re模块

相关重要方法:都是从一个字符串中匹配你要的规则

- findall
- search
- match

> findall 返回所有匹配规则的元素放入到列表里

```
import re
# 匹配所有a
res = re.findall('a','eva egon yuan')
print(res) # ['a','a']

res = re.findall('[a-z]+','eva egon yuan')
print(res) # ['eva','egon','yuan']
```

> search 

- 从前往后找,匹配到一个就返回
- 而且需要你调用group()才能返回结果
- 但如果没有找到结果，那么返回的是None,调用group会爆错

```
import re

res = re.search('a','eva egon yuan')
print(res.group()) #  'a'


res = re.search('j','eva egon yuan')
print(res) # None
print(res.group()) #  None.group() 报错 报错  

正确的写法

res = re.search('j','eva egon yuan')
if res:
    print(res.group()) 
```

> match 用法和search是一样的

- match是从头开始匹配，如果正则从头开始匹配上，就返回一个变量
- 匹配的内容调用group才能显示
- 如果没匹配上，就返回None,调用group报错

```

res = re.match('e','eva egon yuan')
if res:
    print(res.group()) 

结果：
'e'

res = re.match('a','eva egon yuan')
if res:
    print(res.group()) 

结果： 什么也不打印


res = re.match('[a-z]','eva egon yuan')
if res:
    print(res.group()) 

结果：
'eva'
```

> split

```
res = re.split('[ab]','abcd')
# 先按a分割得到 ''和'bcd' 再按b分割 '','','cd'
print(res)
结果：

['','','cd']
```

> sub

```
res = re.sub('\d', 'H', 'eva3egon4yuan4', 1)
#将数字替换成'H'，参数1表示只替换1个
print(res) #evaHegon4yuan4

res = re.sub('\d', 'H', 'eva3egon4yuan4')
print(res) #evaHegonHyuanH
```

> subn

返回元组(替换的结果,替换了多少次)

```
res = re.subn('\d', 'H', 'eva3egon4yuan4')
# 将数字替换成'H'，返回元组(替换的结果,替换了多少次)
print(res) # ('evaHegonHyuanH', 3)
```

> compile

- 将正则表达式编译成为一个 正则表达式对象
- 正则表达式对象调用正则方法

```
obj = re.compile('\d{3}')  #将正则表达式编译成为一个 正则表达式对象，规则要匹配的是3个数字
res = obj.search('abc123eeee') #正则表达式对象调用search，参数为待匹配的字符串
print(res.group())  
#结果 ： 123
```

> finditer

- 返回的是一个迭代器
- 迭代元素要调用group才能查看

使用它就是为了节省内存

```
ret = re.finditer('\d', 'ds3sy4784a')   #finditer返回一个存放匹配结果的迭代器
print(ret)  # <callable_iterator object at 0x10195f940>
print(next(ret).group())  #查看第一个结果
print(next(ret).group())  #查看第二个结果
print([i.group() for i in ret])  #查看剩余的左右结果
```

#### 为什么search match要通过group才可以获取结果

分组的概念

```
res = re.search('^[1-9](\d{14})(\d{2}[0-9x]?$','110105199912122277')

print(res.group())
print(res.group(1))
print(res.group(2))
```

#### 注意点「?」的第三种用法

1. 在量词的地方表示匹配0次或一次
2. 在量词后面表示惰性匹配的标志
3. 放到分组的第一个代表取消分组优先

> 引入findall没有 group的概念所有它就优先匹配「()」组里的

```
import re

ret = re.findall('www.(baidu|oldboy).com', 'www.oldboy.com')
print(ret)  # ['oldboy']     这是因为findall会优先把匹配结果组里内容返回,如果想要匹配结果,取消权限即可

ret = re.findall('www.(?:baidu|oldboy).com', 'www.oldboy.com')
print(ret)  # ['www.oldboy.com']
```

> split的优先级查询

- split也没有分组的机制，所以如果有分组就分组优先

```
ret=re.split("\d+","eva3egon4yuan")
print(ret) #结果 ： ['eva', 'egon', 'yuan']

ret=re.split("(\d+)","eva3egon4yuan")
print(ret) #结果 ： ['eva', '3', 'egon', '4', 'yuan']

#在匹配部分加上（）之后所切出的结果是不同的，
#没有（）的没有保留所匹配的项，但是有（）的却能够保留了匹配的项，
#这个在某些需要保留匹配部分的使用过程是非常重要的。
```

#### 其他规则

```
re.I(IGNORECASE)忽略大小写，括号内是完整的写法
re.M(MULTILINE)多行模式，改变^和$的行为
re.S(DOTALL)点可以匹配任意字符，包括换行符
re.L(LOCALE)做本地化识别的匹配，表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境，不推荐使用
re.U(UNICODE) 使用\w \W \s \S \d \D使用取决于unicode定义的字符属性。在python3中默认使用该flag
re.X(VERBOSE)冗长模式，该模式下pattern字符串可以是多行的，忽略空白字符，并可以添加注释
```