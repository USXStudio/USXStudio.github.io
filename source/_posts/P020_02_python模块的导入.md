---
title: P020_02_python模块的导入
date: 2018-07-29 19:41:20
tags: fullstack
---

### 模块

> 很多时候，你发现你写的代码有很多重复内容这个时候你就要使用模块了

#### 模块的导入



在同一目录下我们这样操作

1. 新建模块文件 demo.py

```
内容如下：
print('in demo.py')

a = 'hello'

def read():
    print('haha'+a)
```

2. 新建调用模块的文件aa.py

```
import demo.py

#执行aa.py你会发现打印了 in demo.py 

如果你多次导入demo模块呢？
import demo.py
import demo.py
import demo.py
import demo.py

# 依然只打印一次  in demo.py 
```

3. 如果调用demo.py里的方法

- 找到模块
- 创建这个模块的命名空间
- 把文件中的内容放到命名空间里

```
import demo # 这句话的时候就是把demo.py的内容加载到内存里，同时demo里所有的内容只能通过demo这个命名空间来调用
demo.read() # 
demo.a
```

> 为什么一个模块不会被重复导入

- 当你导入一个模块的时候，会先去sys.modules里查看是否已经导入了这个模块
- 如果没被导入，就依据sys.path路径去寻找模块找到了正常运行，没找到报错
- 找到了就导入
- 创建这个模块的命名空间
- 把文件中的内容放到命名空间里

```
import sys
print(sys.modules.keys()) # 查看导入的模块
```

#### 给模块其一个别名

```
import time as t
t.time()
```

#### 导入多个模块

```
import sys,time,random,os
```

#### 模块引入的一些编码规则

```
1. 先引入内置的
2. 扩展的如django
3. 自己实现的
```

#### from import的使用方式

```
之前是
import time
time.sleep()

你可以这样
from time import sleep
sleep()
```

> 回归之前的demo.py

```
from demo import read
# 此时无法访问demo.py里的 a变量 因为你单独导入了read方法
# 但是read方法里访问了 a变量 ，说明引入模块的时候会把a保存在内存里只是本模块访问不到 


# 如果你在引入的地方定义同名read方法,那么导入的read方法就被覆盖
def read():
    pass
```

> #### 另一种 from import *

但是这种非常不安全，不建议使用

```
import demo from *
# 相当于把里面的东西全部引入进来,可以不带命名空间之间使用
read()
print(a)
```

> #### 最后一种方式导入模块

1. 我们又新建一个模块demo2.py

```
__all__ = ['a']
print('in demo2.py')

a = 'hello'

def read():
    print('haha'+a)
```

2. 引入新建的模块demo2

```
import demo2 from *
read() # 报错 

# 因为在模块内部设置了 __all__
__all__是一个列表代表 import demo2 from *的形式导出的内容

因为刚刚只设置了 __all__ = ['a'] 
因此只有 a可用
```

双下all变量只跟 *关联

> #### 最后一个需求知识点

```
1.因为demo2.py里有一句 print('in demo2.py') 
2.所以在你导入demo2模块的时候 程序会打印 in demo2.py
3.需求就是 只在demo2.py里打印不在 被导入的文件里 打印这句话
```

- 双下name

demo2.py里添加如下内容,并执行,发现打印 

```
a = 'hello'
def read():
    print('haha'+a)

print(__name__) # __main__
```

如果在别的地方导入demo2

```
import demo2

# 打印 demo2
```

> 完成需求只在本模块执行相关代码

```
a = 'hello'
def read():
    print('haha'+a)

if __name__ == '__main__':
    print('in demo2.py')
```

小结：

```
在模块xxx内部有一个变量 __name__
当我们直接运行这个模块xxx的时候 __name__ == '__main__'
当我们在其他文件内,引入这个模块xxx的时候,这个模块中的 __name__ =='模块的名字'
```



