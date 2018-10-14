---
title: Py002-02-10常用模块九(logging)
date: 2018-08-22 21:44:06
tags: M02
---

### logging (日志模块)

logging的日志可以分为5个级别

- debug()
- info()
- warning()
- error()
- critical()

#### 最简单用法

```
import logging

logging.warning("user [alex] attempted wrong password more than 3 times")
logging.critical("server is down")

输出

WARNING:root:user [alex] attempted wrong password more than 3 times
CRITICAL:root:server is down

# 默认以root执行的，就是你没指定用户的情况下
```

其中下面这句中的level=loggin.INFO意思是，把日志纪录级别设置为INFO，也就是说，只有比日志是INFO或比INFO级别更高的日志才会被纪录到文件里，在这个例子， 第一条日志是不会被纪录的，如果希望纪录debug的日志，那把日志级别改成DEBUG就行了。

#### 如果想把日志写到文件里，也很简单

- filename 日志写入的文件
- level 输入指定的级别以及以上级别的信息

当你再次运行的时候，日志会追加而不是覆盖

```
import logging

logging.basicConfig(filename='example.log',level=logging.INFO)
logging.debug('This message should go to the log file')
logging.info('So should this')
logging.warning('And this, too')

# example.log内容如下
INFO:root:So should this
WARNING:root:And this, too
```

#### 自定义日志格式

日志起码要有时间吧！

```
import logging
logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
logging.warning('is when this event was logged.')

#输出
12/12/2010 11:46:36 AM is when this event was logged.
```

除了加时间，还可以自定义一大堆格式，下表就是所有支持的格式

```
%(name)s	Logger的名字
%(levelno)s	数字形式的日志级别
%(levelname)s	文本形式的日志级别
%(pathname)s	调用日志输出函数的模块的完整路径名，可能没有
%(filename)s	调用日志输出函数的模块的文件名
%(module)s	调用日志输出函数的模块名
%(funcName)s	调用日志输出函数的函数名
%(lineno)d	调用日志输出函数的语句所在的代码行
%(created)f	当前时间，用UNIX标准的表示时间的浮 点数表示
%(relativeCreated)d	输出日志信息时的，自Logger创建以 来的毫秒数
%(asctime)s	字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896”。逗号后面的是毫秒
%(thread)d	线程ID。可能没有
%(threadName)s	线程名。可能没有
%(process)d	进程ID。可能没有
%(message)s	用户输出的消息
```

#### 日志同时输出到屏幕和文件

如果想同时把log打印在屏幕和文件日志里，就需要了解一点复杂的知识 了

Python 使用logging模块记录日志涉及四个主要类，使用官方文档中的概括最为合适：

- logger提供了应用程序可以直接使用的接口；
- handler将(logger创建的)日志记录发送到合适的目的输出；
- filter提供了细度设备来决定输出哪条日志记录；
- formatter决定日志记录的最终输出格式。

