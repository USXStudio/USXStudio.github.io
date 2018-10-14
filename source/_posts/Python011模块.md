---
title: Python011模块
date: 2018-06-18 15:26:49
tags: python
---

### 模块

```
import 模块名
# 引入模块
import random
random.randint(0,2)
```

#### 自定义模块

```
# test.py内容如下

def test:
    print('test')


# 在你的模块里引入并使用
import test
test.test()
```

> 导入模块的过程实际就是把模块里的内容执行一遍

你的模块需要自测，但是不想在被引入的地方调用

```
__name__  代表模块的名字 test.py

单独执行test.py  
print(__name__)  # __main__
在其他模块里导入 test.py
会打印  __test__
```

> 实现自测才调用的效果

```
def test():
    print('haha')

#测试
if __name__ == "__main__":
    print(__name__)
    test()
```

### 模块导入路径问题

```
import random 
random.randint(1,3)
# 每次都有random命名空间是不是很不爽


# 太极端会把random里所有方法导入进来
from random import *
randint(1,3)


# 单独导入一个
from random import randint
randint(1,3)

# 导入多个
from random import randint,randrange
```

### 定位模块

> 如果第三方模块在当前执行目录则没有关系，如果模块拆的比较碎同时不在同一目录，就会有路径问题

1. 当前目录
2. 如果不在当前目录，搜索shell改变了pythonpath下的每个目录
3. 如果不在以上目录，python会查看默认路径 UNIX下默认路径为/usr/local/lib/python
4. 模块搜索路径存储在system模块的sys.path变量里。它是一个列表，包含以上目录

```
import sys
# 将你的模块路径插入到最前面
sys.path.insert(0,"test.py的路径") 
# 将你的模块路径插入到最后， 如果加载失败一定是 sys.path里有同名的test模块覆盖了你的test
# sys.path.append("test.py的路径") 
def test():
    print('haha')
```

### all变量

```
模块中的__all__
```

当你有test.py文件

```
class Test(object):
    def test(def):
        print("class defined method")

def test1():
    print("test1")

def test2():
    print("test2")
```

```
from test import *
这时你可以调用test模块所有东西
```

> 在test.py里添加如下内容后

```
__all__ = ["Test","test1"]
```
```
from test import *
这时你可以调用test模块里的 Test类 和 test1 
调用 test2报错  
```

> all仅仅影响的是 from test import * 里的 「*」

```
from test import *
from test from test2
test1 ()
test2() #不报错
```



