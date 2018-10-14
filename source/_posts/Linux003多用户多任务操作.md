---
title: Linux003多用户多任务操作
date: 2018-06-14 21:33:48
tags:
---

### 多用户

一个电脑多个用户可以同时登录

### 多任务

一个电脑可以跑很多程序 QQ 微信 浏览器 QQ音乐

### ifconfig

查看网卡信息(一般能看到俩个代表你的虚拟机能上网了)

```
ifconfig | grep '192' 查找一大堆文档中的关键字
```

#### vmware虚拟机的网络设置

```
NAT模式  比如你的window能上网，你想让虚拟机借助它上网(对于交换机来说它只看到一台电脑window)
桥接模式  你想让window上网，而且虚拟机也能上网而不是间接的通过window就用这个(对于交换机它看到了两台电脑)
仅主机模式 在没有网的情况下和windows进行通讯

```

### ping 

测试是否能通信

```
ping 192.168.2.123
```

### ssh远程登录

比如阿里云服务器跟这效果一样

```
ssh hjx@192.168.17.76 代表远程登录服务器

如果你第一次登录会让你确认是否连接  ==> yes
然后让你输入密码如果成功了就能远程登录 并且对系统进行远程操作
```

### who命令

查看当前登录的用户信息

```
如果你的电脑被攻击了
who可以查看当前谁登录了
```

whoami当前终端的用户

```
whoami
```

exit退出登录

```
exit
```

### 添加用户账号useradd

```
-m 自动创建家目录
-d 路径  指定家目录 
useradd aaa -m -d /home/aaa
创建用户 aaa 同时指定家目录为 aaa
通常提示你权限不够

sudo useradd aaa -m -d /home/aaa
会提示你输入这个用户的密码
```

> 修改密码passwd

```
passwd aaa
可能会提示权限不够

sudo passwd aaa
请输入新密码
```

### su切换用户

```
su aaa 
输入密码
su hjx
输入密码
```

问题 如果你切换了多个用户然后exit会怎样

```
su aaa
su bbb
su ccc
su ddd

你当前在 ddd用户 然后输入exit 退出会在 ccc用户
```

```
su aaa 切换后你当前目录在哪还在哪
su - aaa 切换后跳到家目录
```

### userdel删除用户

```
userdel abc 删除abc用户但不删除用户的主目录 (一般删除这个)
userdel -r abc 删除abc用户同时删除用户的主目录
```

### sudo添加权限

```
sudo -s 一段时间内不询问你密码(最高权限)

以$结尾代表普通用户
以#结尾代表root用户
```

### 用户组的查看

1. cat etc/group
2. groupmod + 三次tab键

什么是用户组

```
比如你输入 ls -al后显示如下信息
-rw-r--r-- root root 
第一个root代表用户
第二个root代表用户组
如果其他用户也在 root用户组那他就有 root用户的权限
```

#### 添加用户组

```
groupadd YYY
groupdel YYY

权限不够请加sudo
```

#### 修改用户所在用户组

```
-g 代表你将来默认的组(主组)
-G 代表向其他组添加用户

usermod -g 用户组 用户名 

usermod -g aaa bbb 把bbb用户修改到aaa用户组里
```

一个用户可以有多个用户组

```
sudo usermod -a -G XXX bbb 将 bbb添加到 XXX组里
```

### chmod修改文件权限

```
drwxrwxrwx 
第一个rwx代表文件拥有者(用户)的权限
第二个rwx代表同组者里的(用户)的权限
第三个rwx代表其他人的权限

r代表读
w代表写
x代表执行
-代表没有权限
```

chmod的选项

```
u 代表该文件的所有者user
g 代表用户组的用户group
o 代表其他 other
a 代表所有人 all
+ 增加权限
- 减少权限
= 把当前权限改为现在的权限   =左边是u/g/o/a =右边是 r/w/x
```

示例

```
chmod u+x 1.txt 用户增加执行权限
chmod g-w 1.txt 同组者去掉写入权限
chmod u=rwx 1.txt
```

#### 数字代表权限

```
r读 ==> 对应数字4
w写 ==> 对应数字2
x执行 ==> 对应数字1

chmod 444  1.txt 可读权限
chmod 761 1.txt 用户=可读可写可执行 用户组=可读可写 其他用户=可执行权限


chmod 777 aa 只会修改aa文件夹的权限
chmod 777 aa -R aa目录里的东西全部权限
```

### cal和date查看时间（自行百度）

### ps查看进程

```
ps
ps -aux 显示全部信息

PID代表程序运行的进程号
```

### kill终止进程

```
kill pid
kill 进程号
```

#### 关机重启

```
reboot 重启
shutdown -r now 重启 但是会给用户提示
shutdown -h now 立刻关机
shutdown -h 20:25 系统在 20:25关机
shutdown -h +10 10分钟后关机
```

### 查看磁盘占用 du

```
du -h
```

### 用文本编辑器gedit