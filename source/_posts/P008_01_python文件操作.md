---
title: P008_01_python文件操作
date: 2018-07-22 17:10:01
tags: fullstack
---

### 文件处理

#### 1.只读 和 读写

> 'r' 只读模式【默认模式，文件必须存在，不存在则抛出异常】 

```
f = open('a.txt','r',encoding='utf-8')
data = f.read()
f.close()
```

> ‘r+' 读写【可读，可写】

```
f = open('a.txt','r+',encoding='utf-8')
data = f.read()
f.close()
```

#### 2.只写 和 写读：

> 'w' 只写模式【不可读；不存在则创建；存在则清空内容】

```
f = open('log','w',encoding='utf-8')
f.write('aaa')
f.close()
```

> 'w+' 写读【可读，可写】

```
f = open('log','w+',encoding='utf-8')
f.write('aaa')
print(f.read())
f.close()
```

#### 3.追加 和 追加写

> 'a' 之追加写模式【不可读；不存在则创建；存在则只追加内容】

```
f = open('log','a',encoding='utf-8')
f.write('aaa')
f.close()
```

> 'a+' 写读【可读，可写】

```
f = open('log',mode='a+',encoding='utf-8')
f.write('aaa')
print(f.read())
f.close()
```

#### 4.只写 和 写读

```
'x'    只写模式【不可读；不存在则创建，存在则报错】

'x+'    写读【可读，可写】
```

#### 5.rb、wb、ab、xb的用法：

1. 对于非文本文件，我们只能使用b模式，"b"表示以字节的方式操作（而所有文件也都是以字节的形式存储的，使用这种模式无需考虑文本文件的字符编码、图片文件的jgp格式、视频文件的avi格式）

2. 以b方式打开时，读取到的内容是字节类型，写入时也需要提供字节类型，不能指定编码

> b模式写入的时候如果是汉字就得转换为进制码

```
f = open('log',mode='rb')
f.read() # 进制码
f.close()


f = open('log',mode='wb')
f.write('纠纷'.encode('utf-8'))
f.close()
```

#### 6.操作文件的方法：

```
f.read() #读取所有内容,光标移动到文件末尾
f.readline() #读取一行内容,光标移动到第二行首部
f.readlines() #读取每一行内容,存放于列表中

f.write('1111\n222\n') #针对文本模式的写,需要自己写换行符
f.write('1111\n222\n'.encode('utf-8')) #针对b模式的写,需要自己写换行符
f.writelines(['333\n','444\n']) #文件模式
f.writelines([bytes('333\n',encoding='utf-8'),'444\n'.encode('utf-8')]) #b模式

#seek 光标移动到第几个字节的位置
f.seek()    移动到最开始
f.seek(2)   

#tell    告诉你光标的位置
f.tell()

#readable    #文件是否可读
f.readable()
```

#### 7.truncate的用法：

```
truncate是截断文件，所有文件的打开方式必须可写，但是不能用w或w+等方式打开，因为那样直接清空文件了，所以truncate要在r+或a或a+等模式下测试效果
```

#### 8.强大装逼打开和关闭文件结合代码：

```
with open('log',mode='r+',encoding='utf-8') as f：
    f.read()
# 自动调用close()
```

对俩文件执行读写操作

```
with open('log',mode='r+',encoding='utf-8') as f1, open('log',mode='w+',encoding='utf-8') as f2：
```

> 注意实现 with后的读写操作必须在with的缩进内


#### 9.修改文件

> 需求：实现文件的编辑效果

```
with open('小护士班主任',encoding='utf-8') as f,open('小护士班主任.bak','w',encoding='utf-8') as f2:
    for line in f:
        if '星儿' in line:  #班主任:星儿
            line = line.replace('星儿','啊娇')
        #写文件
        f2.write(line) #小护士:金老板

import os
os.remove('小护士班主任') #删除文件
os.rename('小护士班主任.bak','小护士班主任')  #重命名文件
```

#### 总结

> 打开文件

```
#open('路径'，'打开方式'，'指定编码方式')
# 打开方式 r w a r+ w+ a+ b
    #r+ 打开文件直接写 和读完再写
# 编码方式 —— utf-8
```

> 操作文件


- 读

```
read 一次性读
readlines 一次性读
readline 一行一行读
    不知道在哪儿结束
    视频 图片 rb bytes 按照字节读
    
for循环 —— 最好！！！
```

- 写

```
write
```

- 光标 —— 文件指针

```
seek _ 指定光标移动到某个位置
tell _ 获取光标当前的位置
truncate _ 截取文件
```

> 关闭文件

- close