---
title: Linux002难点命令
date: 2018-06-13 23:07:48
tags:
---

### ln软链接和硬链接

```
# 软链接，就是快捷方式  (源文件没了，现在的链接文件都不好使)
# 修改源文件或链接文件操作的都是一个文件
ln -s 源文件 链接文件 


# 硬连接，就算删除了源文件不会影响硬链接的查看
# 修改源文件或链接文件操作的都是一个文件
ln 源文件 链接文件
```

### cat 查看文件的内容

```
cat 1.txt
```

将一个文件的内容填充到一个文件里

```
cat 1.txt > 2.txt

# step1 cat 1.txt 复制 1.txt的内容
# step2 将1.txt的内容复制到 2.txt
```

复制多个文件

```
cat 1.txt 2.txt > 3.txt 将 1.txt和2.txt的内容复制到 3.txt

```

### grep 文本搜索

从文件中搜索内容

```
# 如1.txt的内容
abcdefg
Abcdefg
------1----m
------2----n
------3----m
alown
txt
txt
```

```
-v显示不包含匹配文本的所有行
-n代表匹配内容的行号
-i 忽略大小写

grep -n 'a' 1.txt 打印所有带a的打印出来

grep -n '^a' 1.txt 打印所有以a开头的打印出来

grep -n 'a$' 1.txt 打印所有以a结尾的打印出来
```

### help

查看帮助文档

```
ls --help
```

### man

是linux提供的手册

```
man ls
```

### 历史命令

```
按上键
或者 history
history将历史命令列举出来
```

### find查找文件

从指定路径查找文件名(条件同样支持正则)

```
find ./ -name test.sh 查找当前目录下所有名为 test.sh的文件
find ./ -name .sh 查找当前目录下所有后缀为 .sh的文件
find ./ -name "[A-Z]" 查找当前目录下所有以[A-Z]大写字母开头的文件
find  /tmp -size 2M  查找/tmp目录下所有等于2M的文件
find  /tmp -size +2M  查找/tmp目录下所有大于2M的文件
find  /tmp -size -2M  查找/tmp目录下所有小于2M的文件
find  ./ -size +4k -size -5M  查找当前目录下所有大于4k小于5m的文件
find  ./  -perm 0777  查找当前目录下权限为777文件或目录
```

### cp 拷贝文件 mv剪切文件

```
cp 源文件  目标文件

选项
-v 显示进度
-r 递归的复制
-i 交互式复制（你确认复制吗？）
-f 强制复制
-a 在复制目录时使用，保留链接/文件属性并递归的复制目录，保留文件原有的属性
(比如失效的软链接文件也会被复制)

递归的赋值目录
cp -r 源文件目录  目标文件目录

cp a b
cp a/* b

mv a b
mv a/* b
```

### 压缩和解压缩

tar打包

```
-c 生成档案文件，创建打包文件
-v 代表进度
-f 指定档案文件名称f后面一定是.tar文件

tar -cvf test.tar * 把当前路径所有东西打包到 test.tar里
```

tar打包

```
tar -xvf test.tar 把打包文件还原
```


gzip 对已经打包的文件进行压缩

```
gzip test.tar 
就会产生 test.tar.gz 文件(打包压缩后的)

gzip  test.tar test.tar.gz 跟上面的一样效果(麻烦)


gzip -d test.tar.gz 解压缩  ==> test.tar
```

简单的方式打包压缩

```
tar -zcvf test.tar.gz * 把当前目录的东西 打包压缩到 test.tar.gz文件里

tar -zxvf test.tar.gz  把test.tar.gz文件解压缩并且解除打包
```

bzip2压缩解压

```
tar -jcvf aa.tar.bz2 *
tar -jxvf aa.tar.bz2
```

> 注意：这几种压缩解压不能混用，即gzip压缩后只能用gzip解压缩


zip 压缩解压

```
zip  aa.tar.bz2 *
unzip  aa.tar.bz2
```

### which查看命令位置

```
which ls 
```

