---
title: Py002-02-03常用模块二(random,str,sys,os)
date: 2018-08-21 09:50:12
tags: M02
---

### random 模块

- randint(1-100) 左闭右开
- randrange(1-100) 左闭右闭 
- random() 随机浮点数
- choise() 从字符串和列表里随机选取一个
- sample('随机内容',个数) 从字符串里 随机取几位 返回列表

```
# 从 1-100直接取随机数
random.randint(1,100) # 不包含100

random.randrange(1,100) # 包含100

# 随机浮点数
random.random()

# 从 字符串里返回随机的字符 也可以是列表
random.choise('fdsa342423')

# 
random.sample('afsd25afa321sf',3) # ['f','a','5']
```

### string 模块

```
import string 

string.digits # 返回数字
string.hexdigits # 十六进制的  0123456789ABCDEF
string.ascii_letters # 大小写字母返回
string.ascii_lowercase # 小写字母
```

> 通常用来生成验证码

```
import random
import string


s = string.ascii_lowercase + string.digits
''.join(random.sample(s),5)
```

> 洗牌模式

将有序的内容打乱

```
d = list(range(100))
random.shuffle(d) # 这样 d 就乱序了
```

### os 模块

[常用api](https://www.cnblogs.com/sunyang945/p/7900957.html)

```
#os模块就是对操作系统进行操作，使用该模块必须先导入模块：
import os

#getcwd() 获取当前工作目录(当前工作目录默认都是当前文件所在的文件夹)
result = os.getcwd()
print(result)

#chdir()改变当前工作目录
os.chdir('/home/sy')
result = os.getcwd()
print(result)

open('02.txt','w')

#操作时如果书写完整的路径则不需要考虑默认工作目录的问题,按照实际书写路径操作
open('/home/sy/下载/02.txt','w')

#listdir() 获取指定文件夹中所有内容的名称列表
result = os.listdir('/home/sy')
print(result)

#mkdir()  创建文件夹
#os.mkdir('girls')
#os.mkdir('boys',0o777)

#makedirs()  递归创建文件夹
#os.makedirs('/home/sy/a/b/c/d')

#rmdir() 删除空目录
#os.rmdir('girls')

#removedirs 递归删除文件夹  必须都是空目录
#os.removedirs('/home/sy/a/b/c/d')

#rename() 文件或文件夹重命名
#os.rename('/home/sy/a','/home/sy/alibaba'
#os.rename('02.txt','002.txt')

#stat() 获取文件或者文件夹的信息
#result = os.stat('/home/sy/PycharmProject/Python3/10.27/01.py)
#print(result)

#system() 执行系统命令(危险函数)
#result = os.system('ls -al')  #获取隐藏文件
#print(result)

#环境变量
'''
环境变量就是一些命令的集合
操作系统的环境变量就是操作系统在执行系统命令时搜索命令的目录的集合
'''
#getenv() 获取系统的环境变量
result = os.getenv('PATH')
print(result.split(':'))

#putenv() 将一个目录添加到环境变量中(临时增加仅对当前脚本有效)
#os.putenv('PATH','/home/sy/下载')
#os.system('syls')

#exit() 退出终端的命令

#os模块中的常用值
#curdir  表示当前文件夹   .表示当前文件夹  一般情况下可以省略
print(os.curdir)

#pardir  表示上一层文件夹   ..表示上一层文件夹  不可省略!
print(os.pardir)

#os.mkdir('../../../man')#相对路径  从当前目录开始查找
#os.mkdir('/home/sy/man1')#绝对路径  从根目录开始查找

#name 获取代表操作系统的名称字符串
print(os.name) #posix -> linux或者unix系统  nt -> window系统

#sep 获取系统路径间隔符号  window ->\    linux ->/
print(os.sep)

#extsep 获取文件名称和后缀之间的间隔符号  window & linux -> .
print(os.extsep)

#linesep  获取操作系统的换行符号  window -> \r\n  linux/unix -> \n
print(repr(os.linesep))



#导入os模块
import os

#以下内容都是os.path子模块中的内容

#abspath()  将相对路径转化为绝对路径
path = './boys'#相对
result = os.path.abspath(path)
print(result)

#dirname()  获取完整路径当中的目录部分  &  basename()获取完整路径当中的主体部分
path = '/home/sy/boys'
result = os.path.dirname(path)
print(result)

result = os.path.basename(path)
print(result)

#split() 将一个完整的路径切割成目录部分和主体部分
path = '/home/sy/boys'
result = os.path.split(path)
print(result)

#join() 将2个路径合并成一个
var1 = '/home/sy'
var2 = '000.py'
result = os.path.join(var1,var2)
print(result)

#splitext() 将一个路径切割成文件后缀和其他两个部分,主要用于获取文件的后缀
path = '/home/sy/000.py'
result = os.path.splitext(path)
print(result)

#getsize()  获取文件的大小
#path = '/home/sy/000.py'
#result = os.path.getsize(path)
#print(result)

#isfile() 检测是否是文件
path = '/home/sy/000.py'
result = os.path.isfile(path)
print(result)

#isdir()  检测是否是文件夹
result = os.path.isdir(path)
print(result)

#islink() 检测是否是链接
path = '/initrd.img.old'
result = os.path.islink(path)
print(result)

#getctime() 获取文件的创建时间 get create time
#getmtime() 获取文件的修改时间 get modify time
#getatime() 获取文件的访问时间 get active time

import time

filepath = '/home/sy/下载/chls'

result = os.path.getctime(filepath)
print(time.ctime(result))

result = os.path.getmtime(filepath)
print(time.ctime(result))

result = os.path.getatime(filepath)
print(time.ctime(result))

#exists() 检测某个路径是否真实存在
filepath = '/home/sy/下载/chls'
result = os.path.exists(filepath)
print(result)

#isabs() 检测一个路径是否是绝对路径
path = '/boys'
result = os.path.isabs(path)
print(result)

#samefile() 检测2个路径是否是同一个文件
path1 = '/home/sy/下载/001'
path2 = '../../../下载/001'
result = os.path.samefile(path1,path2)
print(result)


#os.environ 用于获取和设置系统环境变量的内置值
import os
#获取系统环境变量  getenv() 效果
print(os.environ['PATH'])

#设置系统环境变量 putenv()
os.environ['PATH'] += ':/home/sy/下载'
os.system('chls')

# 获取当前终端大小
os.get_terminal_size()

# 杀死进程
import signal # 信号模块
os.kill(10884,signal.SIGKILL)

```

> 判断文件是否存在

```
import os
os.path.exists(test_file.txt)
```

### sys 模块

```
sys.argv  # 命令行参数，第一个元素就是程序本身
sys.exit(n) # 退出程序，正常退出时exit(0)
sys.version # python解释器版本信息
sys.maxint # 最大int值
sys.path # 返回模块搜索路径
sys.platform # 操作系统平台名称
sys.stdout.wtite('please') # 标准输出，
val = sys.stdin.readline()[:-1] # 标准输入
sys.getrecursionlimit # 递归最大层数
sys.setrecursionlimit(1200) # 设置递归层数
sys.getdefaultencoding # 解释器编码
sys.getfilesstemencoding # 获取内存数据到文件的默认编码

```