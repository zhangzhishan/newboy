---
title: '软件调试的艺术(The Art of Debugging with GDB, DDD, and Eclipse)'
tags: debug
categories: Linux
date: 2016-01-19 21:58:48
---

*   [调试的原则][1]

*   [主要调试器操作][2]

	*   [单步调试源代码][3]

		*   [设置断点][4]

		*   [删除和禁用断点][5]

		*   [检查变量][6]

		*   [设置监视点][7]

*   [P.S. 命令缩写][8]

## 调试的原则

*   调试的本质：确认原则
*   从简单工作开始调试
*   使用自顶向下的方法
*   使用调试工具确定段错误的位置
*   通过发出中断确定无限循环的位置
*   使用二分搜索

## 主要调试器操作

### 单步调试源代码

break 设置断点

condition breakpointnumber condition 条件断点

break 30 if num\_y==1 条件断点

clear bplinenumber 删除断点

info break 查询要找的断点编号

next 执行下一行

step 在函数调用时进入函数

continue 恢复执行并继续

tbreak 临时断点

### 设置断点

break line\_number

break function

break filename:line\_number

break filename:function

break +offset

break -offset

### 删除和禁用断点

delete breakpoint\_list

delete 删除所有断点

clear 清除GDB将执行的下一个指令处的断点，这种方法适用于要删除GDB已经到达的断点的情况。

clear function, clear filename:funciton, clear line\_number, clear filename:line\_number 根据位置清除断点

disable breakpoint\_list

enable breakpoint\_list

enable once breakpoint\_list 在断点下次引起GDB暂停执行后被禁用

### 检查变量

print 命令

### 设置监视点

watch z

watch (z &gt; 29)

## P.S. 命令缩写

b 表示 break

i b 表示 info break

cond 表示 condition

r 表示 run

n 表示 next

s 表示 step

c 表示 continue

bt 表示 breaktrace

[1]:	#toc_0
[2]:	#toc_1
[3]:	#toc_2
[4]:	#toc_3
[5]:	#toc_4
[6]:	#toc_5
[7]:	#toc_6
[8]:	#toc_7