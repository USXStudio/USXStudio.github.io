---
title: Py003-04-02struct模块
date: 2018-09-25 11:24:29
tags: M03
---

### struct模块

这里我们只用它的打包和解包

```

import struct
str = '0123456789'
str_size = len(str) # 10

# 打包
str_key = struct.pack('i', str_size)
print('打包',str_key)

# 解包
data_size = struct.unpack('i', str_key)
print('解包',data_size)

‘’‘
打包 b'\n\x00\x00\x00'
解包 (10,)
’‘’
```

[他人博客链接](https://www.cnblogs.com/leomei91/p/7602603.html)

[他人博客链接](https://www.cnblogs.com/volcao/p/8807507.html)


