---
title: Py003-04-01optparse模块
date: 2018-09-25 11:17:36
tags: M03
---

### optparse模块

获取执行py文件时获取的参数

> 如test.py文件如下

```
import optparse

parser = optparse.OptionParser()
parser.add_option("-s","--server", dest="server", help="ftp server ip_addr")
parser.add_option("-P","--port",type="int", dest="port", help="ftp server port")

a,b = parser.parse_args()

print(a,b)


# 如果执行时参数如下
# python3 test.py a b c -s localhost -P 9999
# 结果如下
# {'server': 'localhost', 'port': 9999} ['a','b','c']
```
