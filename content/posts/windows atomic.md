---
title: windows atomic in C++
tag: cpp
date: 2019-04-15 15:16:42
---

## Differences between `InterlockedIncrement64` and `InterlockedIncrementNoFence64`

`InterlockedIncrement64` generates a full memory barrier (or fence) to ensure that memory operations are completed in order.

`InterlockedIncrementNoFence64` generates no memory barriers (or fences) and does not guarantee that independent memory operations before or after it are completed in order.
> [呼呼呼山]()(http://code4fun.me)
> 2019-04-15 15:16:42
