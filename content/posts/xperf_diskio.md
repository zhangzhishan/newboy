---
title: Xperf diskio action[8/15]
tag: performance, xperf
date: 2020-06-23 14:03:19
---

可以使用的命令在[官方文档](https://docs.microsoft.com/en-us/windows-hardware/test/wpt/diskio)里面都包含了,很简单的一条命令, `-a diskio [-util <n>] [-summary] [-counts] [-detail] [-overlap] [-range T1 T2]`, 参数也就五个:

* `-util [n]` disk utilization report, 其中 n 为这个 report 的时间间隔, 默认是 1s.
* `-summary` 输出 I/O summary report
* `-detail` 输出每个 I/O event
* `-overlap` 输出 I/O events 的 overlapping 关系, 也即当多线程执行的时候, 各个 IO 的开始与结束周期.
* `-range` T1 T2 把输出限制到 [T1, T2) 这个时间范围里面, 单位是微秒.

默认的 report 就是  disk utilization report.

这个就是官方的所有文档了, 但是 report 是什么样子, 以及其中每个column的含义并没有详细的介绍. 下面通过一些例子来说明这个的使用.

## examples
### -util
我们可以在默认的不加任何参数的情况下使用, `-util` 获得所有disk分时间的使用率.

```
xperf -i D:\xperflog\xperf_trace_log.etl -tle -o d:\disk_usage.txt -a diskio -util
```

如下图, 时间的单位是微秒, 所以也就是每 1s 各个硬盘的读写和使用率的统计信息, 下图有四组 Read, Write, Usage 对应着四块硬盘.

![[20200628140316905_1946.png]]

根据这些信息, 我们可以方便的统计在一段时间内, 硬盘使用率变化情况, 以及不同硬盘间的区别.

![[20200628141230255_20839.png]]

上图就反应了两块硬盘以RAID0形式组成一个 volume 在一个时间段内进行大规模的读的时候的 disk usage 变化图. 我们可以发现随时间变化, disk usage越来越少, 而且下降频率越来越慢, 同时两块盘的 usage 基本保持一致, 也即两边负载相同. 这时候我们其实是可以去探究一下,为什么disk usage会在负载不变的情况下逐渐降低.

### -summary
summary 是按照文件来做的统计信息. 在一个给定或者全部的时间内, 每一个文件进行的读写的次数, 以及总共的 IO time.

```
xperf -i D:\xperf_trace_log.etl -tle -o diskio_summary.txt -a diskio -summary -range 368000000 866000000
```

![[20200628142002577_12105.png]]

一个典型的`-summary` 的output 如上图所示, 前三列对应总的 IO 次数(Count), IO 大小(Size, 单位是 bytes), IO 硬盘服务的时间 (SrvT, 单位是微秒), 后面紧跟着的是读(R)操作和写(W)操作各自的信息, 然后就是总的IO 所用的时间 (IOTime). 根据这个, 我们可以进一步分析每个文件的读写 IO 占比是多少, 是否符合预期, 以及其平均的 IO 时间是多少, 是否和硬盘所能提供的性能差不多, 平均一次 IO 处理多少数据, 是否过多或者过少.

### -detail
detail  包含所有单次 IO 的信息, 把所有 IO 发生的时候的情况按照时间顺序排列下来.
```
xperf -i D:\xperf_trace_log.etl  -tle -o diskio_detail.txt  -a diskio -detail -range 368000000 370000000
```
一个输出结果如下图所示, 第一列就是 IO Type 有可能是 DiskWrite 也有可能是 DiskRead. 然后就是这次 IO 的开始时间和结束时间, 以及两种之差, 也就是这次 IO 的时间, 接下来是硬盘服务的时间, 以及该次 IO 的大小, 然后就是这次 IO 作用于硬盘上位置偏移量. Pri 指的是 IO 的优先级, 后面几个在报告里给出了解释.

* QD/I - Number of outstanding IOs at IO start time. IO 开始的时候的 queuelength
* QD/C - Number of outstanding IOs at IO Completion time. IO 结束的时候的 queuelength
* IBCB - Number of IOs that initialized before start time and completed before end time. 当前IO 开始前初始化且在当前IO结束之前结束的IO数量.
* IBCA - Number of IOs that initialized before start time and completed after end time. 当前IO 开始前初始化且在当前IO结束之后才结束的IO数量.
* IACB - Number of IOs that initialized after start time and completed before end time. 当前IO 开始后才初始化且在当前IO结束之前就结束的IO数量.

然后就是 issue 该IO的 process 的名字及PID, 以及该IO 对应的硬盘和文件名.

![[20200628142949777_32343.png]]

这里的信息是更进一步细节化的, 我们可以从中发现一些IO 的野值情况, 也可以发现单次IO是否稳定之类的信息, 如下图蓝色的线和橘色的线分别对应着不同 IO event中 IOTime 和 Disk SrvT的变化曲线.

![[20200628143602516_2846.png]])

### -overlap
overlap 顾名思义是表示着不同的 IO 之间的交叠关系, 其大部分信息和detail 中的是相同的, 主要是把 IBCB等参数给具象化了, 你不但可以知道有几个这样的IO 还可以知道是哪些IO.

```
xperf -i D:\xperf_trace_log.etl -tle -o diskio_overlap.txt  -a diskio -overlap -range 368000000 370000000
```

结果类似于下图这样, 他会对每个disk产生单独的数据,会在最开始标明是针对于哪个disk的, 在最左侧就是该IO 的event, `+` 表示 IO 的开始 `-` 表示结束, 而 `|` 表示仍在继续进行. 别的都可以根据文件名自行理解到, 其中 `IrpFlags` 是当前IO产生的时候我们给的一些 flag的组合, 其中 `Irp` 是 `IO request packet`的缩写, 其基本内容定义在 `wdm.h` 中,

![[20200628144519909_19384.png]])

其各个flag 的含义如下:

```cpp
#define IRP_NOCACHE                     0x00000001
#define IRP_PAGING_IO                   0x00000002
#define IRP_MOUNT_COMPLETION            0x00000002
#define IRP_SYNCHRONOUS_API             0x00000004
#define IRP_ASSOCIATED_IRP              0x00000008
#define IRP_BUFFERED_IO                 0x00000010
#define IRP_DEALLOCATE_BUFFER           0x00000020
#define IRP_INPUT_OPERATION             0x00000040
#define IRP_SYNCHRONOUS_PAGING_IO       0x00000040
#define IRP_CREATE_OPERATION            0x00000080
#define IRP_READ_OPERATION              0x00000100
#define IRP_WRITE_OPERATION             0x00000200
#define IRP_CLOSE_OPERATION             0x00000400
#define IRP_DEFER_IO_COMPLETION         0x00000800
#define IRP_OB_QUERY_NAME               0x00001000
#define IRP_HOLD_DEVICE_QUEUE           0x00002000
#define IRP_RETRY_IO_COMPLETION         0x00004000
#define IRP_CLASS_CACHE_OPERATION       0x00008000

#define IRP_QUOTA_CHARGED                 0x01
#define IRP_ALLOCATED_MUST_SUCCEED        0x02
#define IRP_ALLOCATED_FIXED_SIZE          0x04
#define IRP_LOOKASIDE_ALLOCATION          0x08
```

那么这里的 `0x062901` 也就很容易发现是由 `IRP_NOCACHE, IRP_DEFER_IO_COMPLETION, IRP_READ_OPERATION, IRP_HOLD_DEVICE_QUEUE` 这些 flag组合起来的, 说明这是一个不使用 cache, 直接读取内容的 raw io.

> [呼呼呼山](http://code4fun.me)
> 2020-06-23 14:03:19
