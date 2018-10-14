---
title: Py001_01_04Python逻辑运算和分支语句和循环
date: 2018-08-13 20:40:41
tags: M01
---

### 逻辑运算

- 算数运算

```
+
- 
*
/
%  取余
** 幂运算   2的三次方    ==> 2**3
// 整除     11//2 ==>5
```

- 比较运算

```
== 等于
!= 不等于
<> 不等于 类似 !=  在python3里废除了
> 
<
>=
<= 
```

- 赋值运算

```
=
+=
-=
*=
/=
%=
**=
//=
```

- 逻辑运算

```
and 且
or  或
not 非
```

### 分支语句

> 单分支

```
if 条件 :
    满足条件后执行的代码

age = 56
if age > 50:
    print('岁数太大不要')
print('haha')

结果：
'岁数太大不要'
'haha'
```

> 双分支

```
if 条件 :
    满足条件后执行的代码
else:
    不满足条件执行的代码
```

> 多分支

```
if 条件1 :
    print('条件1合适')
elif 条件2 :
    print('条件2合适')
elif 条件3 :
    print('条件3合适')
elif 条件4 :
    print('条件4合适')
else :
    print('都不合适')
```

### 循环

- while

```
while 条件:
    执行代码
```

> 打印0-100的数字

```
count = 0
while count<=100:
    print(count)
    count += 1
print('loop is end')
```

> 打印0-100的偶数

```
count = 0
while count<=100:
    if count % 2 == 0:
        print("loop",count)
    count += 1
```

> 打印0-100，第50次不打印，第60-80打印对应平方

```
count = 0
while count<=100:
    if count == 50:
        pass # 代表过
    elif count>=60 and count<=80:
        print(count*count)
    else:
        print(count)
    count += 1
```

#### deal loop(死循环)

```
# 就是条件永远都满足，就会触发死循环

while True:
    print('xxx')

```

#### continue 和 break

- break 用于完全结束一个循环，跳出循环体执行后面的语句
- continue 终止本次循环，接着还执行后面的循环

```
count = 0
while count < 100:
    print(count)
    if count == 5:
        break
    count += 1

count = 5的时候结束循环

但是如果把break 换成continue就会造成死循环

因为 count +=1 得不到执行 count一直等于5
```

#### 你没见过的while ... else ...

> while后面还有个else

意思是：当while没有被break终止的话，就会执行else里的语句

```
count = 0
while count < 100:
    count += 1
    print(count)
    if count == 5:
        break
else:
    print('程序被break中断过')
```