---
title: Object_Oriented_Programming
date: 2021-07-24 11:33:07
tags: Object Oriented 
---

## *args|**kwargs

`args` 是 `arguments` 的缩写，表示位置参数；

`kwargs` 是` keyword arguments` 的缩写，表示关键字参数

` Python` 中可变参数的两种形式

并且 `*args` 必须放在` **kwargs` 的前面

因为位置参数在关键字参数的前面。

`*args`就是就是传递一个可变参数列表给函数实参，这个参数列表的数目未知，甚至长度可以为0

`**kwargs`则是将一个可变的关键字参数的字典传给函数实参，同样参数列表长度可以为0或为其他值。

`args`和`kwargs`组合起来可以传入任意的参数，这在参数未知的情况下是很有效的，同时加强了函数的可拓展性

`*`:解包

## 装饰器【Decorator】

### 作用

1. 增加代码可读性
2. 给function加上一些功能，比如加上时间计时器
3. 装饰器本质上是一个函数|类【利用函数闭包：函数nested函数】
4. 减少重复操作
5. Decorator装饰的函数（define function而不是instance）

### Example

```python
# 输出所有的不大于num的素数
def is_prime(num):
    for item in range(1, num+1):
        if item % 2 != 0:
            print(item, end=' ')
            
>>检测is_prime函数的运行时间

import time

def is_prime(num):
    t1 = time.time()
    for item in range(1, num+1):
        if item % 2 != 0:
            print(item, end=' ')
    t2 = time.time()
    print('\n', 'total time:{:.4} s'.format(t2-t1))

>>增加可读性


# 增加装饰器decorator
def display_time(func):
    # 如果需要装饰的func中需要添加参数，则在wrapper中添加*args，并且func同样添加
    def wrapper(*args):
        t1 = time.time()
        func(*args)
        t2 = time.time()
        print('\n', 'total time:{:.4} s'.format(t2 - t1))
    return wrapper


@display_time
def is_prime(num):
    for item in range(1, num+1):
        if item % 2 != 0:
            print(item, end=' ')
            
is_prime(10000)
>>1~9999
total time：0.03494 s
```

