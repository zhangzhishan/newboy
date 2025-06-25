---
title: cdecl 简介
tag: cpp
date: 2019-03-26 09:36:55
---

## cdecl

| 要点 | __cdecl | __stdcall | __fastcall |
| --- | --- | --- | --- |
| 参数传递方式 | 右 -> 左 | 右 -> 左 | 左边开始的两个不大于 4 字节（DWORD）的参数分别放在 ECX 和 EDX 寄存器，其余的参数自右向左压栈传送 |
| 清理栈方 | 调用者清理 | 被调用函数清理 | 被调用函数清理 |
| 适用场合 | C/C++、MFC 的默认方式; 可变参数的时候使用; | Win API | 要求速度快 |
| C 编译修饰约定 | _functionname | _functionname@number | @functionname@number |