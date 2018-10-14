---
title: Py002-02-04常用模块三(shutil)
date: 2018-08-21 10:32:43
tags: M02
---

### shutil 模块

复制压缩文件的模块

[详细操作参考](https://www.cnblogs.com/wupeiqi/articles/4963027.html)

> #### 复制文件

```
# shutil.copyfileobj(fsrc, fdst[, length])

import shutil

f1 = open('1.txt',mode="r")
f2 = open('2.txt',mode="w")

shutil.copyfileobj(f1,f2)
```

> 递归的操作目录

```
# 递归复制目录及文件
shutil.copytree('aa','cc')

# 递归的删除目录
shutil.rmtree('cc')

# 递归的移动文件 剪切功能
shutil.move('aa','bb')
```

> 压缩打包

```
shutil.make_archive(base_name, format,...)
```

- base_name： 压缩包的文件名，也可以是压缩包的路径。只是文件名时，则保存至当前目录，否则保存至指定路径，
如：www                        =>保存至当前路径
如：/Users/wupeiqi/www =>保存至/Users/wupeiqi/
- format：	压缩包种类，“zip”, “tar”, “bztar”，“gztar”
- root_dir：	要压缩的文件夹路径（默认当前目录）
- owner：	用户，默认当前用户
- group：	组，默认当前组
- logger：	用于记录日志，通常是logging.Logger对象

```
# 将 bb 下的文件打包放置当前程序目录
 
import shutil
ret = shutil.make_archive("bbgz", 'gztar', root_dir='aa')
 
 
# 将 bb 下的文件打包放置 aa/aagz 目录
import shutil
ret = shutil.make_archive("aa/aagz", 'gztar', root_dir='aa')
```
