---
title: P016_02_python_memoryview用途
date: 2018-07-27 12:33:25
tags: fullstack
---

> ## 注意：非重点知识点(几乎用不到)

### memoryview

- bytearray是可变(mutable)的字节序列，相对于Python2中的str，但str是不可变(immutable)的。
- 在Python3中由于str默认是unicode编码，所以只有通过bytearray才能按字节访问。

> memoryview为支持buffer protocol[1,2]的对象提供了按字节的内存访问接口，好处是不会有内存拷贝。

默认str和bytearray支持buffer procotol。

#### 下面两种行为的对比：

> 简单点就是，str和bytearray的切片操作会产生新的切片str和bytearry并拷贝数据，使用memoryview之后不会。

##### 1.不使用memoryview

```
>> a = 'aaaaaa'
>> b = a[:2]    # 会产生新的字符串

>> a = bytearray('aaaaaa')
>> b = a[:2]    # 会产生新的bytearray
>> b[:2] = 'bb' # 对b的改动不影响a
>> a
bytearray(b'aaaaaa')
>> b
bytearray(b'bb')
```

##### 2.使用memoryview

```
>> a = 'aaaaaa'
>> ma = memoryview(a)
>> ma.readonly  # 只读的memoryview
True
>> mb = ma[:2]  # 不会产生新的字符串

>> a = bytearray('aaaaaa')
>> ma = memoryview(a)
>> ma.readonly  # 可写的memoryview
False
>> mb = ma[:2]      # 不会会产生新的bytearray
>> mb[:2] = 'bb'    # 对mb的改动就是对ma的改动
>> mb.tobytes()
'bb'
>> ma.tobytes()
'bbaaaa'
```


#### 我的使用场景是网络程序中socket接收和接收数据的解析：

1. 使用memoryview之前的sock接收代码简化如下

```
def read(size):
    ret = '' 
    remain = size
    while True:
        data = sock.recv(remain)
        ret += data     # 这里不断会有新的str对象产生
        if len(data) == remain:
            break
        remain -= len(data)
    return ret
```

2. 使用meoryview之后，避免了不断的字符串拼接和新对象的产生

```
def read(size):
    ret = memoryview(bytearray(size)) 
    remain = size
    while True:
        data = sock.recv(remain)
        length = len(data)
        ret[size - remain: size - remain + length] = data
        if len(data) == remain:
            break
        remain -= len(data)
    return ret
```

> 返回memoryview还有一个优点，在使用struct进行unpack解析时可以直接接收memoryview对象，非常高效（避免大的str进行分段解析时大量的切片操作）。

例如：

```
mv = memoryview('\x00\x01\x02\x00\x00\xff...')
type, len = struct.unpack('!BI', mv[:5])
...
```