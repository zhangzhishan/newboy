---
title: Fibers
tag: cpp
date: 2019-04-16 11:25:21
---

[fibers api](https://docs.microsoft.com/en-us/windows/desktop/ProcThread/fibers)

A Fiber is a lightweight thread that uses cooperative multitasking instead of preemptive multitasking. A running fiber must explicitly "yield" to allow another fiber to run, which makes their implementation much easier than kernel or user threads.

A Coroutine is a component that generalizes a subroutine to allow multiple entry points for suspending and resuming execution at certain locations. Unlike subroutines, coroutines can exit by calling other coroutines, which may later return to the point where they were invoked in the original coroutine.

A Green Thread is a thread that is scheduled by a virtual machine (VM) instead of natively by the underlying operating system. Green threads emulate multithreaded environments without relying on any native OS capabilities, and they are managed in user space instead of kernel space, enabling them to work in environments that do not have native thread support.


## Fiber Local Storage
A fiber can use fiber local storage (FLS) to create a unique copy of a variable for each fiber. If no fiber switching occurs, FLS acts exactly the same as thread local storage. The FLS functions (FlsAlloc, FlsFree, FlsGetValue, and FlsSetValue) manipulate the FLS associated with the current thread. If the thread is executing a fiber and the fiber is switched, the FLS is also switched.

To clean up the data associated with a fiber, call the DeleteFiber function. This data includes the stack, a subset of the registers, and the fiber data. If the currently running fiber calls DeleteFiber, its thread calls ExitThread and terminates. However, if the selected fiber of a thread is deleted by a fiber running in another thread, the thread with the deleted fiber is likely to terminate abnormally because the fiber stack has been freed.

根据上面的描述我们可以发现,由于`TlsAlloc`函数没有带有free clalback的版本,而在没有fiber switch的情况下,FLS和TLS是等价的,所以可以用`FlsAlloc`在一定程度上代替其.

> [呼呼呼山]()(http://code4fun.me)
> 2019-04-16 11:25:21
