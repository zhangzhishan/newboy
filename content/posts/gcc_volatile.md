---
title: warning implicit dereference will not access object of type volatile
tag: cpp
date: 2019-04-08 17:15:45
---

根据[gcc 官方文档](https://gcc.gnu.org/onlinedocs/gcc-4.0.4/gcc/Volatiles.html)，这种情况的出现是由于:

When using a reference to volatile, G++ does not treat equivalent expressions as accesses to volatiles, but instead issues a warning that no volatile is accessed. The rationale for this is that otherwise it becomes difficult to determine where volatile access occur, and not possible to ignore the return value from functions returning volatile references. Again, if you wish to force a read, cast the reference to an rvalue. 

> [呼呼呼山]()(http://code4fun.me)
> 2019-04-08 17:15:45
