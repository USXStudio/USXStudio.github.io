---
title: P019_02_python常用模块二
date: 2018-07-29 11:52:58
tags: fullstack
---

### 时间模块

- 字符串：给人看的 2018-07-29
- 时间戳时间：float时间：计算机看的
- 结构化时间：元组，计算用的

#### 常用方法

1. time.sleep(secs) (线程)推迟指定的时间运行。单位为秒。
2. time.time() 获取当前时间戳

#### strftime 格式化时间 

```
print(time.strftime("%Y-%m-%d %H:%M:%S")) # year month day hour minute second

# 2018-07-29 12:01:20
```

> 时间格式化规则

```
%y 两位数的年份表示（00-99）
%Y 四位数的年份表示（000-9999）
%m 月份（01-12）
%d 月内中的一天（0-31）
%H 24小时制小时数（0-23）
%I 12小时制小时数（01-12）
%M 分钟数（00=59）
%S 秒（00-59）
%a 本地简化星期名称
%A 本地完整星期名称
%b 本地简化的月份名称
%B 本地完整的月份名称
%c 本地相应的日期表示和时间表示
%j 年内的一天（001-366）
%p 本地A.M.或P.M.的等价符
%U 一年中的星期数（00-53）星期天为星期的开始
%w 星期（0-6），星期天为星期的开始
%W 一年中的星期数（00-53）星期一为星期的开始
%x 本地相应的日期表示
%X 本地相应的时间表示
%Z 当前时区的名称
%% %号本身
```

#### 结构化时间

> 时间元组:localtime将一个时间戳转换为当前时区的struct_time
```
time.localtime()
# time.struct_time(tm_year=2018, tm_mon=7, tm_mday=29, tm_hour=12, tm_min=5, tm_sec=42, tm_wday=6, tm_yday=210, tm_isdst=0)
```

#### 不同时间格式的转换

![](https://images2015.cnblogs.com/blog/827651/201707/827651-20170724144151992-1508626640.png)

```
#时间戳-->结构化时间
#time.gmtime(时间戳)    #UTC时间，与英国伦敦当地时间一致
#time.localtime(时间戳) #当地时间。例如我们现在在北京执行这个方法：与UTC时间相差8小时，UTC时间+8小时 = 北京时间 
>>>time.gmtime(1500000000)
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=14, tm_hour=2, tm_min=40, tm_sec=0, tm_wday=4, tm_yday=195, tm_isdst=0)
>>>time.localtime(1500000000)
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=14, tm_hour=10, tm_min=40, tm_sec=0, tm_wday=4, tm_yday=195, tm_isdst=0)

#结构化时间-->时间戳　
#time.mktime(结构化时间)
>>>time_tuple = time.localtime(1500000000)
>>>time.mktime(time_tuple)
1500000000.0

#结构化时间-->字符串时间
#time.strftime("格式定义","结构化时间")  结构化时间参数若不传，则现实当前时间
>>>time.strftime("%Y-%m-%d %X")
'2017-07-24 14:55:36'
>>>time.strftime("%Y-%m-%d",time.localtime(1500000000))
'2017-07-14'

#字符串时间-->结构化时间
#time.strptime(时间字符串,字符串对应格式)
>>>time.strptime("2017-03-16","%Y-%m-%d")
time.struct_time(tm_year=2017, tm_mon=3, tm_mday=16, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=3, tm_yday=75, tm_isdst=-1)
>>>time.strptime("07/24/2017","%m/%d/%Y")
time.struct_time(tm_year=2017, tm_mon=7, tm_mday=24, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=0, tm_yday=205, tm_isdst=-1)
```

> asctime 

![](https://images2015.cnblogs.com/blog/827651/201707/827651-20170724144235883-1963884021.png)

```
#结构化时间 --> %a %b %d %H:%M:%S %Y串
#time.asctime(结构化时间) 如果不传参数，直接返回当前时间的格式化串
>>>time.asctime(time.localtime(1500000000))
'Fri Jul 14 10:40:00 2017'
>>>time.asctime()
'Mon Jul 24 15:18:33 2017'

#时间戳 --> %a %d %d %H:%M:%S %Y串
#time.ctime(时间戳)  如果不传参数，直接返回当前时间的格式化串
>>>time.ctime()
'Mon Jul 24 15:19:07 2017'
>>>time.ctime(1500000000)
'Fri Jul 14 10:40:00 2017' 
```

> ### 计算时间差

```
import time
true_time=time.mktime(time.strptime('2017-09-11 08:30:00','%Y-%m-%d %H:%M:%S'))
time_now=time.mktime(time.strptime('2017-09-12 11:00:00','%Y-%m-%d %H:%M:%S'))
dif_time=time_now-true_time
struct_time=time.gmtime(dif_time)
print('过去了%d年%d月%d天%d小时%d分钟%d秒'%(struct_time.tm_year-1970,struct_time.tm_mon-1,
                                       struct_time.tm_mday-1,struct_time.tm_hour,
                                       struct_time.tm_min,struct_time.tm_sec))
```

### random模块

```
import random
#随机小数
random.random()     
 # 大于0且小于1之间的小数 0.7664338663654585
random.uniform(1,3)
#大于1小于3的小数 1.6270147180533838

#随机整数
random.randint(1,5)  # 大于等于1且小于等于5之间的整数
random.randrange(1,10,2) # 大于等于1且小于10之间的奇数


#随机选择一个返回
random.choice([1,'23',[4,5]])  
# #1或者23或者[4,5]

#随机选择多个返回，返回的个数为函数的第二个参数
random.sample([1,'23',[4,5]],2)
# #列表元素任意2个组合 [[4, 5], '23']


#打乱列表顺序
item=[1,3,5,7,9]
random.shuffle(item) 
# 打乱次序
item # [5, 1, 3, 7, 9]

random.shuffle(item)
# item [5, 9, 7, 1, 3]
```

> 生成随机验证码

```
import random

def v_code():

    code = ''
    for i in range(5):

        num=random.randint(0,9)
        alf=chr(random.randint(65,90))
        add=random.choice([num,alf])
        code="".join([code,str(add)])

    return code

print(v_code())
```