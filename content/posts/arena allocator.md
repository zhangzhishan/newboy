---
title: Something related to arena allocator
tag: cpp
date: 2019-05-07 12:10:42
---

[A good post](http://jm.taobao.org/2011/06/22/983/)

Some highlights:

> 它的原理：

> 创建一个大小固定的bytes数组和一个偏移量，默认值为0。
> 
> 分配对象时，将新对象的data bytes复制到数组中，数组的起始位置是偏移量，复制完成后为偏移量自增data.length的长度，这样做是防止下次复制数据时不会覆盖掉老数据（append）。
> 
> 当一个数组被充满时，创建一个新的数组。
> 
> 清理时，只需要释放掉这些数组，即可得到固定的大块连续内存。
> 
> 在Arena Allocation方案中，数组的大小影响空间连续性，越大内存连续性越好，但内存平均利用率会降低。

这样做GC的时候就只需要处理多个区域而不是所有的内存空间.

> [呼呼呼山](http://code4fun.me)
> 2019-05-07 12:10:42
