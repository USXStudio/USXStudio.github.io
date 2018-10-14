---
title: P015_02_python内置函数二
date: 2018-07-26 20:43:11
tags: fullstack
---

### 字符串类型代码的执行

- eval 
- exec
- compile

```
eval('print(123)') # 123
exec('print(123)') # 123

eval('1+2+3') # 10
exec('1+2+3') # None
```

> eval 和 exec区别

- 都可以执行字符串代码
- exec没有返回值

> #### eval建议不要轻易使用，只能用在你明确知道要执行的代码

尤其eval(文件里的字符串/用户输入的) 非常危险

> compile编译

```
code = '''for i in range(10):
    print(i)
'''
exec(code) # 
```


> 如果code的内容是500行代码 每次exec都要编译

> compile就是一次编译多次执行

```
code1 = 'for i in range(10):print(i)'
compile1 = compile(code1,'','exec')
exec(compile1)


code1 = '1+2+3'
compile2 = compile(code2,'','eval')
print(eval(compile2)) # 6
```

> pycharm没法帮你找到字符串里的变量声明

> #### 交互类命令要用 single

```
code3 = 'name = input("请输入账号")'
compile3 = compile(code3,'','single')
print(exec(compile3)) # 执行交互命令
print(name) # pycharm无法检测到name 因为它在待编译的模板里还没有执行
```

### 和数字相关

- complex 复数
- bool
- int 
- float 当你小数位过多时就不准了

> 进制转换

- bin() 二进制 0b开头
- oct() 八进制 0o开头
- hex() 十六进制 0x开头

> 数学运算

- abs() 绝对值
- divmod()  div除法 mod取余 ==》 分页处理
- round() 四舍五入
- pow() 幂运算
- sum()
- min()
- max()

```
abs(-5) # 5
divmod(7,2) # (7整除2,7对2取余) =>(3,1)
divmod(9,5) # (9整除5,9对5取余) =>(1,4)
round(3.1415926,3) # 3.14

pow(2,3) # 2*2*2 ==》8
如果三个参数就是 对最后的参数取余
pow(2,3,3) # 8取余3 ==》2


sum(1,2,3) # 6
sum([1,2,3]) # 6
sum([1,2,3],10) # 10代表起始值  ==》 31

min([1,2,3,4]) # 1
min(1,2,3,4) # 1
min(1,2,3,-4) # -4
min(1,2,3,-4,key=abs) # 1
```