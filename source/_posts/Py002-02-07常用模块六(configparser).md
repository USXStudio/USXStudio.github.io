---
title: Py002-02-07常用模块六(configparser)
date: 2018-08-21 22:27:02
tags: M02
---

### configparser 配置解析模块

此模块用于生成和修改常见配置文档，当前模块的名称在 python 3.x 版本中变更为 configparser。

来看一个好多软件的常见配置文件格式如下

```
[DEFAULT]
ServerAliveInterval = 45   
Compression = yes
CompressionLevel = 9
ForwardX11 = yes

[bitbucket.org]
User = hg

[topsecret.server.com]
Port = 50022
ForwardX11 = no
```

> 解析配置文件

```
import configparser

conf = configparser.ConfigParser()

conf.read('conf.ini')

print(conf.sections()) # 打印每个section 但是不打印 default里的
print(conf.default_section)

print(conf['bitbucket.ort']['User']) # hg

print(list(conf['bitbucket.ort'].keys()))
```

> 其它增删改查语法

```
[group1] # 支持的两种分隔符“=”, “:”
k1 = v1
k2:v2

[group2]
k1 = v1
```

```
import configparser

config = configparser.ConfigParser()
config.read('i.cfg')

# ########## 读 ##########
#secs = config.sections()
#print(secs)
#options = config.options('group2') # 获取指定section的keys
#print(options)

#item_list = config.items('group2') # 获取指定 section 的 keys & values ,key value 以元组的形式
#print(item_list)

#val = config.get('group1','key') # 获取指定的key 的value
#val = config.getint('group1','key')

# ########## 改写 ##########
#sec = config.remove_section('group1') # 删除section 并返回状态(true, false)
#config.write(open('i.cfg', "w")) # 对应的删除操作要写入文件才会生效

#sec = config.has_section('wupeiqi')
#sec = config.add_section('wupeiqi')
#config.write(open('i.cfg', "w")) # 


#config.set('group2','k1',11111)
#config.write(open('i.cfg', "w"))

#config.remove_option('group2','age')
#config.write(open('i.cfg', "w"))
```
