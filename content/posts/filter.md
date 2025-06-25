---
title: filter
categories: Python
date: 2014-12-06 22:28:19
tags:
---

<tt class="descname"><span class="highlighted">filter</span></tt><big>(</big>_function_, _iterable_<big>)</big>

Construct an iterator from those elements of _iterable_ for which _function_ returns true. _iterable_ may be either a sequence, a container which supports iteration, or an iterator. If _function_ is <tt class="docutils literal"><span class="pre">None</span></tt>, the identity function is assumed, that is, all elements of _iterable_ that are false are removed.

相当于一个过滤函数，根据function的定义筛选出符合function的值，

例如filter(odd, number)，的返回值就是number中的所有奇数结果。

可以如下定义，

```
import random

def filter(bool_func, seq):
    filtered_seq = []
    for eachitem in seq:
        if bool_func(eachitem):
            filtered_seq.append(eachitem)
    return filtered_seq

def odd(n):
    return n % 2

allNums = [i for i in range(100)]
random.shuffle(allNums)
print(filter(odd, allNums))
```

