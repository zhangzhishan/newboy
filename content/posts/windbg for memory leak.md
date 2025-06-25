---
title: Use windbg to check memory leak
tag: wiki
date: 2020-04-10 11:50:34
---
From:
http://blogs.msdn.com/b/paullou/archive/2011/06/28/debugging-managed-code-memory-leak-with-memory-dump-using-windbg.aspx

```
#Set symbols File >> Symbol File path
SRV*c:\symbols*http://msdl.microsoft.com/download/symbols
```

```
.reload
```

```
.loadby sos clr or .loadby sos mscorwks!
```

```
#optional
!VerifyHeap 
```

```
 #Find largest GC Heapsize - leak is possible if > 1gb or so
 !EEHeap
```

```
 #Find type with the largest size
 !dumpheap -stat
```

```
 #put type here - find type with the largest size
 !dumpheap -type [TYPE]
```

```
#put largest result here
!do [ADDRESS]
```

Loop above as needed, this is where it gets a bit fuzzy.  Look for repeating patterns and high threads

```
#get a count of thread on the thing.  Find a high thread count
!threads
```

```
#Pick a thread to look at it's stack - alternatively you can rum ~*kb for all thread stacks
~[THREAD ID]s
```

```
#show the stack
Kb
```

> [呼呼呼山](http://code4fun.me)
> 2020-04-10 11:50:34
