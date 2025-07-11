---
title: Python 调试指南
tags: Python
categories: Python
date: 2015-09-05 11:44:09
---

# Python 调试指南
很久以前，我的调试都是采用print，由于写的内容比较简单，所以也可以快乐的过下去，但是随着代码量的增加，简单的print调试方法，已经不能很好的解决问题了，再加上听了内核恐慌的调试那一期节目，终于开始了拖了很久的调试的学习，当然Python也有很多的库可以用来进行调试，这里我们只考虑其官方自带的库[pdb](https://docs.python.org/3/library/pdb.html)，本以为是一件很麻烦的事情，可是学习后突然发现，原来是如此的简单，调试一共分为两种模式。

## 1.不引入pdb库，运行时添加参数。

```
python -m pdb test.py
```
这时候就需要用到一些快捷键进行调试，对于稍微有点英语基础的人来说这些还是比较简单的，首先我们让他执行下一行命令，可以是 **n** (next),或者显示当前的代码情况**l**(list),打印变量**p**(print),或者对某些变量进行一些操作（**可以声明变量，但是这有一个坑，就是当你声明一个变量为一些命令字符，例如p的时候，就会出现问题，此时需要采用在变量前面加一！的方式解决**）， 从一个函数中返回**r**(return)，进入一个函数**s**(step into)，继续执行 _c_ （continue），以及破坏性地推出_q_(quit)，以及万能的回车键（会重复上一次执行的命令）。

## 2.引入pdb库，设置断点

有些时候，当函数很长的时候，我们没必要让其从头一步步运行，我们能大致估计出问题出现的位置，此时我们可以设定一个断点。

```python
import pdb
pdb.set_trace()
```
然后正常运行，在断点位置，自动进入调试状况，快捷键依旧和之气的一样。

## 3.常用快捷键总结

| _快捷键_ | _short for_ | _含义_ |
| n | next | 继续执行 |
| b | breakpoint | 设置断点，例如 "b 77"，就是在当前脚本的77行打上断点，还能输入函数名作为参数，断点就打到具体的函数入口，如果只敲b，会显示现有的全部断点 |
| c | continue | 让程序正常运行，直到遇到断点 |
| r | return | 返回执行结果 |
| q | quit | 强制退出 |
| s | step into | 进入函数 |
| p | print | 打印变量 |
| l | list | 输出当前代码状况 |
| cl | clear | 清除断点 |
| h | help | 帮助 |