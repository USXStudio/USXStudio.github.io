---
title: Py003-02-02socket
date: 2018-09-09 13:48:36
tags: M03
---

### socket

#### 简单例子 拨打电话

服务端.py

```
import socket

# 1 买手机
phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
# 2 绑定手机卡

phone.bind(('127.0.0.1',8081)) # ip 加端口号
# 3 开机
phone.listen(5) # 5代表最大链接数

# 4 等电话打入
print('starting....')
conn,client_addr = phone.accept()

# 5 收发消息
# 单位bytes 现在表示最大接受1024字节的数据
data = conn.recv(1024) # 1024代表接受数据的最大数
print('客户端的数据',data)
conn.send(data.upper())

# 6 挂电话
conn.close()

# 7 关机
phone.close()
```

客户端.py

```
import socket

# 1 买手机
phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

# 2 拨号
phone.connect(('127.0.0.1',8081)) # ip 加端口号

# 3 发/收消息
phone.send('hello'.encode('utf-8'))
data = phone.recv(1024)
print(data)

# 4 关闭
phone.close()
```

#### 通讯循环

服务端

```
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.bind(('127.0.0.1',8081)) # ip 加端口号
phone.listen(5) # 5代表最大链接数

print('starting....')
conn,client_addr = phone.accept()

while True: # 通讯循环
    data = conn.recv(1024)
    print('客户端的数据',data)
    conn.send(data.upper())

conn.close()

phone.close()
```

客户端

```
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

phone.connect(('127.0.0.1',8081)) # ip 加端口号

while True:
    msg = input('>>:').strip()
    phone.send(msg.encode('utf-8'))
    data = phone.recv(1024)
    print(data)

phone.close()
```

#### 重复启动服务端的bug

因为端口重复占用，所以必须收到切换端口

```
phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
# 添加这一行代表重用端口
phone.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
phone.connect(('127.0.0.1',8081)) # ip 加端口号
```

#### 客户端直接敲回车发数据的bug

```
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

phone.connect(('127.0.0.1',8081)) # ip 加端口号

while True:
    msg = input('>>:').strip()
    # bug解决
    if not msg:continue
    phone.send(msg.encode('utf-8'))
    data = phone.recv(1024)
    print(data)

phone.close()
```

#### 客户端意外中断，服务器端在linux里会造成死循环的bug

```
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
phone.bind(('127.0.0.1',8081)) # ip 加端口号
phone.listen(5) # 5代表最大链接数

print('starting....')
conn,client_addr = phone.accept()

while True: # 通讯循环
    try:
        data = conn.recv(1024)
        # 针对客户端中断问题
        if not data:break # 适用于linux　如果是windows就 try catch
        print('客户端的数据',data)
        conn.send(data.upper())
    except ConnectionResetError: # 适用于windows
        break

conn.close()

phone.close()
```

#### 在不会并发的情况下——一个一个的服务用户

服务端

```
import socket

phone = socket.socket(socket.AF_INET,socket.SOCK_STREAM)
phone.setsockopt(socket.SOL_SOCKET,socket.SO_REUSEADDR,1)
phone.bind(('127.0.0.1',8081)) # ip 加端口号
phone.listen(5) # 5代表最大链接数

print('starting....')
while True: # 链接循环
    conn,client_addr = phone.accept()

    while True: # 通讯循环
        try:
            data = conn.recv(1024)
            # 针对客户端中断问题
            if not data:break # 适用于linux　如果是windows就 try catch
            print('客户端的数据',data)
            conn.send(data.upper())
        except ConnectionResetError: # 适用于windows
            break
    conn.close()

phone.close()
```

服务一个客户就要把对应客户端终止才能服务下一个客户