---
title: Python024闭包
date: 2018-06-22 13:58:16
tags: python
---

### 闭包

```
def test(number):
    # 内部函数调用访问number
    def test_in():
        print(number)
    # 将内部函数返回
    return test_in


xxx = test(100)
print("-"*30)
xxx(1)
xxx(100)
xxx(200)

```