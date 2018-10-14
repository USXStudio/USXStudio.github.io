---
title: Python005文件操作
date: 2018-06-16 21:37:23
tags: python
---

### 文件操作

1. 打开文件 open

```
f = open('text.txt','w')

# 第二个参数代表模式
- r 读
- w 写 存在则覆盖写入，不存在则创建写入
- a 追加内容，存在则追加，不存在创建写入
不常用的。。。。自行科普
- rb 二进制打开
。。。
```

2. 关闭文件 close

与open成对出现

```
f = open('text.txt','w')

f.close()
```

3. 文件读写

- read 

```
f = open('test.txt')
f.read() # 将文件一次性读完慎用，如果文件5G你内存2G就炸了，真的会爆炸
```

```
如果文件内容是
1111122222333334444455555

# 读5个字节 再次调用会挨着上次的结果继续读
f.read(5) # 11111
f.read(5) # 22222
f.read(5) # 33333
```

- write

```
f = open('abc.txt','w')
f.write('hahahah')
f.close()
```

- readlines

```
f = open('1.txt','r')
f.readlines()

'''
文件有多大也都读取出来，生成一个列表 文件每行占据列表一个元素
[
    'fsdafsdfasdafs\n',
    'fsdafsdfasdafs\n',
    'fsdafsdfasdafs\n',
    'fsdafsdfasdafs\n'
    '\n'
]
'''
```

- readline

```
跟read(5) 类似 它是每次读一行
```

> 稳妥的用法是使用read(1000) 每次读取指定的内容

> 复制一个test.txt 的文件 test[副本].txt

```
# 打开文件
file_read = open('test.txt','r')
file_write = open('test[副本].txt','w')
# 读取写入内容
content = file_read.read()
file_write.write(content)
# 关闭文件
file_read.close()
file_write.close()
```

读写文件的最佳实践

```
#第一种过于粗暴
#content = file_read.read()
#file_write.write(content)

#第二种依然粗暴
#for content in file_read.readlines():
#   file_write.write(content)

#第三种
#content = file_read.read()
#file_write.write(content)
while True:
    content = file_read.readline()
    if len(content)>0:
        file_write.write(content)
    else:
        break
```

### 文件定位

比如你看小说看到了一半被叫走了下次铁定不会从头开始看

- 获取定位 tell

```
f.read(1) # 1
f.read(1) # 2
f.read(1) # 3
f.read(1) # 4
f.read(1) # 5
f.tell()  # 5
```

- seek 自行搜索

### 文件重命名和删除

需要引入模块 os

- 重命名

```
import os
os.rename("1.txt","2.txt") # 将1.txt重命名为2.txt
```

- 删除文件

```
import os
os.remove("1.txt")
```

- 获取当前目录  请参考命令行pwd

```
import os
os.getcwd()
```

- 改变默认目录

```
import os
os.chdir('./aaa') // 切换目录到当前目录的aaa路径下
```

- 获取目录列表

```
import os
os.listdir()
```

- 删除文件夹

```
import os
os.rmdir('./张三')
```
