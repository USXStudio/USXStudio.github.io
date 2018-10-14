---
title: P020_01_python序列化模块
date: 2018-07-29 15:23:05
tags: fullstack
---

### 序列化模块

- 序列化——转向一个字符串的数据类型
- 序列——字符串

数据存储
网络上传输的时候

> #### 序列化：从数据类型==> 字符串的过程

> #### 反序列化：从字符串==>数据类型

#### 重要的几个知识点 「*」代表重要程度 

- json *****
- pickle ****
- shelve ***

##### json通用序列化格式

- 只有很少的一部分数据类型能够通过json转化成字符串

##### pickle

- 所有python里的数据类型都可以转化成字符串格式
- pickle序列化的内容只有python能理解
- 且部分反序列化依赖代码

例如网游 魔兽世界你退出游戏了 但是下次登录你还会在原来的数据基础上。这些信息如血量蓝量背包的物品经验值都是对应一些数据类型并依赖一些模块存在服务器或者你本地

##### shelve

- 序列化句柄
- 使用句柄直接操作非常方便

### json

> 内存相关

- dumps 序列化方法
- loads 反序列化方法



```
import json

dic = {"k1":"v1"}

print(type(dic),dic)
# <class 'dict'> {'k1': 'v1'}

# 序列化
str_d = json.dumps(dic)
print(type(str_d),str_d) 
# <class 'str'> {"k1": "v1"}

# 反序列化
dic_d = json.loads(str_d)
print(type(dic_d),dic_d)
# <class 'dict'> {'k1': 'v1'}
```

> 元组:序列化之后是列表,把元组序列号的内容反序列化后也是列表

> set不能进行序列化：报错

> 文件相关：传递要序列话的内容，和文件句柄

- dump
- load

```
import json

dic = {"k1":"v1"}
f = open('fff','w',encoding='utf-8')
json.dump(dic,f) # 序列化后的字符串写入到文件里
f.close()


f = open('fff',encoding='utf-8')
res = json.load(f)
f.close()
print(res) # {'k1': 'v1'}
```

> 序列化的时候有中文咋办？

读取能正常但是你去看fff文件发现中文已经被转换了 {"k1": "\u4e2d\u56fd"}

```
import json

dic = {"k1":"中国"}
f = open('fff','w',encoding='utf-8')
json.dump(dic,f) # 序列化后的字符串写入到文件里
f.close()


f = open('fff',encoding='utf-8')
res = json.load(f)
f.close()
print(res) # {'k1': '中国'}
```

> 解决dump时 中文会转换为byte编码的问题

- ensure_ascii=False以指定的编码写入

```
import json

dic = {"k1":"中国"}
f = open('fff','w',encoding='utf-8')
json.dump(dic,f,ensure_ascii=False) # 序列化后的字符串写入到文件里
f.close()


f = open('fff',encoding='utf-8')
res = json.load(f)
f.close()
print(res)
```

> #### 注意如果你用json的demp和load只能一次性写入和读取

如果想要分次写入和读取就不能用dump和load

```
# 分次写入
import json
a = [{"k1":1},{"k2":22},{"k3":33}]
f = open('file','w')
for dic in a:
    str_dic = json.dumps(dic)
    f.write(str_dic+'\n')
f.close()

# 分次读取
b = []
f = open('file')
for line in f:
    dic = json.loads(line.strip())
    b.append(dic)
f.close()
```

### pickle

- pickle也有json模块里的 dump/dumps ,load/loads方法
- pickle在dumps/dump的时候你是看不出来的(二进制内容)
- 在调用dump/load 读写文件的时候要使用rb和wb模式



```
import pickle

dic = {"k1":"中国"}
str = pickle.dumps(dic)
print(str) # 一串二进制内容
# b'\x80\x03}q\x00X\x02\x00\x00\x00k1q\x01X\x06\x00\x00\x00\xe4\xb8\xad\xe5\x9b\xbdq\x02s.'
res = pickle.loads(str)
print(res)
# {'k1': '中国'}
```

> 由于pickle序列化之后是二进制内容所以读写的时候要rb和wb模式

```
import pickle
dic = {"k1":"中国"}
f = open('fff3','wb')
pickle.dump(dic,f) # 序列化后的字符串写入到文件里
f.close()


f = open('fff3','rb')
res = pickle.load(f)
f.close()
print(res)
```

> #### pickle的dump和load可以分步序列化写入和读取 (json不行)

```
import pickle
dic = {"k1":"中国"}
dic2 = {"k2":"中国2"}
f = open('fff3','wb')
pickle.dump(dic,f) #
pickle.dump(dic2,f) #
f.close()


f = open('fff3','rb')
res = pickle.load(f)
res2 = pickle.load(f)
f.close()
print(res)
print(res2)
```

### shelve

> shelve也是python提供给我们的序列化工具，比pickle用起来更简单一些。

> shelve只提供给我们一个open方法，是用key来访问的，使用起来和字典类似。

```
import shelve
f = shelve.open('shelve_file')
f['key'] = {'int':10, 'float':9.5, 'string':'Sample data'}  #直接对文件句柄操作，就可以存入数据
f.close()

import shelve
f1 = shelve.open('shelve_file')
existing = f1['key']  #取出数据的时候也只需要直接用key获取即可，但是如果key不存在会报错
f1.close()
print(existing)
```

> 这个模块有个限制，它不支持多个应用同一时间往同一个DB进行写操作。所以当我们知道我们的应用如果只进行读操作，我们可以让shelve通过只读方式打开DB

```
import shelve
f = shelve.open('shelve_file', flag='r')
existing = f['key']
f.close()
print(existing)
```

> 由于shelve在默认情况下是不会记录待持久化对象的任何修改的，所以我们在shelve.open()时候需要修改默认参数，否则对象的修改不会保存。

```
import shelve
f1 = shelve.open('shelve_file')
print(f1['key'])
f1['key']['new_value'] = 'this was not here before'
f1.close()

f2 = shelve.open('shelve_file', writeback=True)
print(f2['key'])
f2['key']['new_value'] = 'this was not here before'
f2.close()
```

writeback方式有优点也有缺点。优点是减少了我们出错的概率，并且让对象的持久化对用户更加的透明了；但这种方式并不是所有的情况下都需要，首先，使用writeback以后，shelf在open()的时候会增加额外的内存消耗，并且当DB在close()的时候会将缓存中的每一个对象都写入到DB，这也会带来额外的等待时间。因为shelve没有办法知道缓存中哪些对象修改了，哪些对象没有修改，因此所有的对象都会被写入。


> #### json补充知识 格式化输出

```
import json
data = {'username':['李华','二愣子'],'sex':'male','age':16}
json_dic2 = json.dumps(data,sort_keys=True,indent=2,separators=(',',':'),ensure_ascii=False)
print(json_dic2)
```