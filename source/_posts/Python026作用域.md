---
title: Python026作用域
date: 2018-06-22 16:10:16
tags: python
---

### 作用域

- globals() 全局的一些东西
- locals()

```
def test():
    a = 11
    b = 22
    print(locals())

test()

# {a:11,b:22}
```

### LEGB规则

> 就是既有全局又有局部的时候变量使用问题(就近原则)

- locals 当前所在命名空间(函数，闭包)，函数的参数也属于命名空间内的变量
- enclosing 外部嵌套函数的命名空间(闭包中常见)

- global
- builtins

查找变量的顺序

locals>enclosing>global>builtins