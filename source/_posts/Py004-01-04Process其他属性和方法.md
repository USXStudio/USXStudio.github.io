---
title: Py004-01-04Process其他属性和方法
date: 2018-10-08 21:38:44
tags: M04
---

### Process其他属性和方法

#### 需求一(父进程等待子进程结束在运行)

- join

```
from multiprocessing import Process
import time,os

def task(name):
    print('%s is running'%name)
    time.sleep(3)
    print('%s is done'%name)

if __name__ == '__main__':
    # args传递的参数必须是元组的形式
    p = Process(target=task,args=('子进程',))

    p.start()

    p.join()
    print('主进程。。。。1',os.getpid(),os.getppid())
    print(p.pid) # 主进程进行完后，子进程pid并不会回收，但是如果主进程结束的时候所有的子进程就会结束
```

#### 不要以代码先后看执行结果

```
from multiprocessing import Process
import time,os

def task(name):
    print('%s is running'%name)

if __name__ == '__main__':
    # args传递的参数必须是元组的形式
    p1 = Process(target=task,args=('子进程1',))
    p2 = Process(target=task,args=('子进程2',))
    p3 = Process(target=task,args=('子进程3',))

    p1.start()
    p2.start()
    p3.start()

    print('主进程开始了')

'''
代码执行结果：
p1.start()只是给操作系统发信号不一定会先执行
p2.start()只是给操作系统发信号不一定会先执行
p3.start()只是给操作系统发信号不一定会先执行
多执行几次就会看大顺序是不确定的
'''
```

> 偏要顺序执行

```
from multiprocessing import Process
import time,os

def task(name):
    print('%s is running'%name)

if __name__ == '__main__':
    # args传递的参数必须是元组的形式
    p1 = Process(target=task,args=('子进程1',))
    p2 = Process(target=task,args=('子进程2',))
    p3 = Process(target=task,args=('子进程3',))

    p1.start()
    p2.start()
    p3.start()

    p1.join()
    p2.join()
    p3.join()
    
    print('主进程开始了')
```

你已经踩坑了
你已经踩坑了
你已经踩坑了

>　假设p1运行1个小时，p2运行半个小时,p3运行10分钟，请问主进程等待多长时间？

```
p1.join() # 让主进程等待，并不会让兄弟进程(其他子进程)等待

所以最后主进程会等待1小时
```


#### 真正意义的串行执行

这样修改就行了

```
p1.start()
p1.join()
p2.start()
p2.join()
p3.start()
p3.join()
```

巧用for循环

```
arr_p = [p1,p2,p3]
for p in arr_p:
    p.start()
    p.join()
```

#### 查看子进程是否活着

- is_alive()

```
from multiprocessing import Process
import time,os

def task(name):
    print('%s is running'%name)
    time.sleep(3)
    print('%s is running'%name)

if __name__ == '__main__':
    # args传递的参数必须是元组的形式
    p = Process(target=task,args=('子进程',))

    p.start()
    print(p.is_alive())
    print('主进程')
    print(p.pid)
    print(p.is_alive())
```

#### 杀死进程

- terminate()

```
from multiprocessing import Process
import time,os

def task(name):
    print('%s is running'%name)

if __name__ == '__main__':
    # args传递的参数必须是元组的形式
    p = Process(target=task,args=('子进程',))

    p.start()
    
    p.terminate() # 只是给操作系统发信号让子进程终止，但不会立即执行 ，所以之后查看子进程is_alive还是True
    print(p.is_alive())
    
    print('主进程')
```

怎样真的死掉

```
p.start()
    
p.terminate() # 只是给操作系统发信号让子进程终止，但不会立即执行
time.sleep(3) #
print(p.is_alive())
```

#### 给子进程起名字

- name

```
# 不指定name参数就有一个默认的
p = Process(target=task,name='sub-Process')
p.start()
print(p.name)
```

#### 父子进程是隔离的

```
from multiprocessing import Process
import time,os

def task():
    global n
    n = 0
    print('子进程内%s'%n)

if __name__ == '__main__':
    # args传递的参数必须是元组的形式
    p = Process(target=task)

    p.start()
    p.join()
    print('主进程',n)
```