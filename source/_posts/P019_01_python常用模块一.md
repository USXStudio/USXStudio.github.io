---
title: P019_01_python常用模块一
date: 2018-07-28 18:59:11
tags: fullstack
---

### 常用模块

- collections(不太重要)
- 时间模块
- random(随机相关)
- os模块(和操作系统打交道的)
- sys模块(和python解释器打交道的)
- 序列化模块(非常重要的)
- re模块(前面已经用了)

### collections模块

1. namedtuple: 生成可以使用名字来访问元素内容的tuple
2. deque: 双端队列，可以快速的从另外一侧追加和推出对象
3. Counter: 计数器，主要用来计数
4. OrderedDict: 有序字典
5. defaultdict: 带有默认值的字典

#### namedtuple

我们知道tuple可以表示不变集合，例如，一个点的二维坐标就可以表示成：

```
p = (1, 2)
```

但是，看到(1, 2)，很难看出这个tuple是用来表示一个坐标的。

这时，namedtuple就派上了用场：

```
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
p.x # 1
p.y # 2
```

> ### 万能公式： namedtuple('名称', [属性list]):

类似的，如果要用坐标和半径表示一个圆，也可以用namedtuple定义：

```
Circle = namedtuple('Circle', ['x', 'y', 'r'])
```

> 扑克牌功能

```
from collections import namedtuple
# namedtuple('扑克牌', ['花色', '数字'])
Card = namedtuple('Card', ['color', 'num'])
c = Card('红桃','2')
```

#### queue 队列 FIFO

> first in first out

```
import queue

q = queue.Queue()
q.put(10)
q.put(1)
q.put(12)

print(q.get()) # 10
print(q.get()) # 1
print(q.get()) # 12
print(q.get()) # 阻塞 队列里只有3个元素 再次进行取值就会阻塞在这里
print(q.get()) # 阻塞 队列里只有3个元素 再次进行取值就会阻塞在这里

print(q.qsize()) # 查看队列里元素个数 取一个少一个
```


例如电影票200张但是500个人买，前200人顺利买了票，后面的只能停在哪里等待票

#### deque 双端队列

使用list存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为list是线性存储，数据量大的时候，插入和删除效率很低。

> deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈：
```
from collections import deque

dq = deque([1,2])
dq.append('a') # 从后面放数据 [1,2,'a']
dq.appendleft('b') # 从前面放数据 ['b',1,2,'a']
dq.insert(2,3)  # 根据索引放数据 ['b','1','3','2','a']
print(dq.pop()) # 从后面取数据 a
print(dq.pop()) # 从后面取数据 2
print(dq.popleft()) # 从前面取数据 b
```

> #### deque除了实现list的append()和pop()外，还支持appendleft()和popleft()，这样就可以非常高效地往头部添加或删除元素。

#### OrderedDict 有序字典

> 使用dict时，Key是无序的。在对dict做迭代时，我们无法确定Key的顺序。

> 如果要保持Key的顺序，可以用OrderedDict：

```
from collections import OrderedDict

d = dict([('a', 1), ('b', 2), ('c', 3)])
# dict的Key是无序的 {'a': 1, 'c': 3, 'b': 2}

od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
od
# OrderedDict的Key是有序的 OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```

> 注意，OrderedDict的Key会按照插入的顺序排列，不是Key本身排序：

```
od = OrderedDict()
od['z'] = 1
od['y'] = 2
od['x'] = 3
od.keys() # 按照插入的Key的顺序返回 ['z', 'y', 'x']
```

#### defaultdict

> 有如下值集合 [11,22,33,44,55,66,77,88,99,90...]，将所有大于 66 的值保存至字典的第一个key中，将小于 66 的值保存至第二个key的值中。

> 即： {'k1': 大于66 , 'k2': 小于66}

原生字典解决方法:很麻烦

```
values = [11, 22, 33,44,55,66,77,88,99,90]

my_dict = {}

for value in  values:
    if value>66:
        if my_dict.has_key('k1'):
            my_dict['k1'].append(value)
        else:
            my_dict['k1'] = [value]
    else:
        if my_dict.has_key('k2'):
            my_dict['k2'].append(value)
        else:
            my_dict['k2'] = [value]
```

defaultdict字典解决方法

```
from collections import defaultdict

values = [11, 22, 33,44,55,66,77,88,99,90]

my_dict = defaultdict(list) # key的默认值为列表

for value in  values:
    if value>66:
        my_dict['k1'].append(value)
    else:
        my_dict['k2'].append(value)
```

> ### 注意事项： 

- defaultdict()的参数必须是callable()的 
- 如果默认值为数字5就不能写defaultdict(5)
- 你可以使用lambda表达式来实现

> 使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict：

```
from collections import defaultdict
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
dd['key1'] # key1存在 'abc'
dd['key2'] # key2不存在，返回默认值 'N/A'
```

#### Counter(计数用的最没用的一个知识点)

> Counter类的目的是用来跟踪值出现的次数。它是一个无序的容器类型，以字典的键值对形式存储，其中元素作为key，其计数作为value。计数值可以是任意的Interger（包括0和负数）。Counter类和其他语言的bags或multisets很相似。

```
c = Counter('abcdeabcdabcaba')
print c
输出：Counter({'a': 5, 'b': 4, 'c': 3, 'd': 2, 'e': 1})
```

[其他详细内容](http://www.cnblogs.com/Eva-J/articles/7291842.html)