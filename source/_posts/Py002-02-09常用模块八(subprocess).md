---
title: Py002-02-09常用模块八(subprocess)
date: 2018-08-22 21:43:01
tags: M02
---

### subprocess 模块

跟操作系统打交道的模块

- run
- call
- Popen 前两种的底层实现

```
import subprocess

a = subprocess.run(['df','-h'])
print(a.args) # 命令的参数
print(a.returncode) # 命令行的返回结果 0代表成功
a.stderr
a.stdout
a.check_returncode() 非 0 报错
```


> 进程   word无法访问 qq的内存数据 不然就会把qq搞崩了， 所以每个应用的内存数据是相互独立的

疑问?  qq里 别人一段消息  你ctrl + c之后 在word里 ctrl + v 数据就过去了？ 因为这是操作系统帮你做的

> #### subprocess 执行时会开启一个进程，那返回的结果  如何获取呢？ 答案是通过操作系统的桥梁

- subprocess.PIPE 就是管道
- check=True 如果不加 执行命令出错了 整个程序没有出错  加了会有报错信息

```
s = subprocess.run(['df','-h'],stderr=subprocess.PIPE,stdout=subprocess.PIPE,check=True)

print(s.stdout)
```

> #### 如果我想执行 带管道的命令呢？  答案是报错  因为subprocess本身就带管道里  如果命令还带就报错

```
# s = subprocess.run(['df','-h','|','grep','disk1'],stderr=subprocess.PIPE,stdout=subprocess.PIPE,check=True)

# 发现还是报错
# s = subprocess.run('df -h |grep disk1',stderr=subprocess.PIPE,stdout=subprocess.PIPE,check=True)

# 添加shell参数 意思是之前是列表里 拼参数  现在你别管了 把整条命令传给 shell执行
s = subprocess.run('df -h |grep disk1', stderr = subprocess.PIPE, stdout = subprocess.PIPE, shell=True)
```

#### call方法

```
#执行命令，返回命令执行状态 ， 0 or 非0
>>> retcode = subprocess.call(["ls", "-l"])

#执行命令，如果命令结果为0，就正常返回，否则抛异常
>>> subprocess.check_call(["ls", "-l"])
0

#接收字符串格式命令，返回元组形式，第1个元素是执行状态，第2个是命令结果 
>>> subprocess.getstatusoutput('ls /bin/ls')
(0, '/bin/ls')

#接收字符串格式命令，并返回结果
>>> subprocess.getoutput('ls /bin/ls')
'/bin/ls'

#执行命令，并返回结果，注意是返回结果，不是打印，下例结果返回给res
>>> res=subprocess.check_output(['ls','-l'])
>>> res
b'total 0\ndrwxr-xr-x 12 alex staff 408 Nov 2 11:05 OldBoyCRM\n'
```

#### Popen()方法

- args：shell命令，可以是字符串或者序列类型（如：list，元组）
- stdin, stdout, stderr：分别表示程序的标准输入、输出、错误句柄
- preexec_fn：只在Unix平台下有效，用于指定一个可执行对象（callable object），它将在子进程运行之前被调用
- shell：同上
- cwd：用于设置子进程的当前目录
- env：用于指定子进程的环境变量。如果env = None，子进程的环境变量将从父进程中继承。

> 下面这2条语句执行会有什么区别？

```
a=subprocess.run('sleep 10',shell=True,stdout=subprocess.PIPE)
a=subprocess.Popen('sleep 10',shell=True,stdout=subprocess.PIPE)
```

区别是Popen会在发起命令后立刻返回，而不等命令执行结果。这样的好处是什么呢？

如果你调用的命令或脚本 需要执行10分钟，你的主程序不需卡在这里等10分钟，可以继续往下走，干别的事情，每过一会，通过一个什么方法来检测一下命令是否执行完成就好了。

Popen调用后会返回一个对象，可以通过这个对象拿到命令执行结果或状态等，该对象有以下方法

- poll()

结论：run方法同步，Popen会新开一个进程不影响主程序，可以通过poll方法获取命令的返回结果



> preexex_fn是什么鬼？代表执行命令前先执行的方法

```
import subprocess

def sayHi():
    print('hello')

a = subprocess.Popen('sleep 10',stdout=subprocess.PIPE,shell=True,preexec_fn=sayHi)
a.stdout
s = a.stdout.read()

print(s)
```

> cwd 用于设置子进程的当前目录

```
a = subprocess.Popen('echo $PWD',cwd='/Users/almost/Desktop/pycode',stdout=subprocess.PIPE,shell=True)
a.stdout
s = a.stdout.read()

print(s) # b'/Users/almost/Desktop/pycode\n'
```

> wait() 等待子进程的结果

> terminate()终止所启动的进程Terminate the process with SIGTERM

> kill() 杀死所启动的进程 Kill the process with SIGKILL

> communicate()与启动的进程交互，发送数据到stdin,并从stdout接收输出，然后等待任务结束

```
>>> a = subprocess.Popen('python3 guess_age.py',stdout=subprocess.PIPE,stderr=subprocess.PIPE,stdin=subprocess.PIPE,shell=True)

>>> a.communicate(b'22')
(b'your guess:try bigger\n', b'')
```

send_signal(signal.xxx)发送系统信号

pid 拿到所启动进程的进程号

