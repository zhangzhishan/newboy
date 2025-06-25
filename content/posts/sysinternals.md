---
title: Windows sysinternals tools
tag: windows
date: 2019-12-25 09:58:00
---

微软在官网上提供了一系列的具有多种用途的小工具, 这里就罗列介绍一部分有用或者有意思的小工具, 详细内容可以参考[官方主页](https://docs.microsoft.com/en-us/sysinternals/downloads/). 分类也基本参照官网, 但是只选取了其中有意思的部分.

## 文件与硬盘相关
### [disk usage](https://docs.microsoft.com/en-us/sysinternals/downloads/du)
一个十分简单的命令行工具,给定路径可以迭代输出路径下所有文件夹的大小. 也可以不迭代或者是给定迭代层数.
![du](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343723_20191225165345511_27540.png)
### [contig](https://docs.microsoft.com/en-us/sysinternals/downloads/contig)
为了优化性能, 我们会对硬盘做一些去碎片化的工作. 而conitg这个工具则主要是针对单个文件的.

Contig is a single-file defragmenter that attempts to make files contiguous on disk. Its perfect for quickly optimizing files that are continuously becoming fragmented, or that you want to ensure are in as few fragments as possible.
### [pendmove](https://docs.microsoft.com/en-us/sysinternals/downloads/pendmoves)
用了 `movefile` 命令可以使得这次操作会在下次启动的时候生效, 而`pendmoves`则可以输出所有待move的文件.
## 网络相关
### [whois](https://docs.microsoft.com/en-us/sysinternals/downloads/whois)
get whois information from whois.markmonitor.com, for example, use `whois64.exe bing.com` can get information below.
![whois](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343717_20191225163408852_17579.png)
### [pipelist](https://docs.microsoft.com/en-us/sysinternals/downloads/pipelist)
列出所有的 named pipes.
## 进程相关
### [vmmap](https://docs.microsoft.com/en-us/sysinternals/downloads/vmmap)
分析给定的进程的虚拟和物理内存, 可以看到每一类虚拟内存和实际物理内存的大小.
![vmmap](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343726_20191225170315138_31452.png)

### [pstools](https://docs.microsoft.com/en-us/sysinternals/downloads/pstools)
pstools 是一个集合, 里面包含各种工具, 主要是这些工具可以方便的在远程机器上执行, 只需要给定机器名和登录所需的用户名密码. 一些参见下面: 

* [psexec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) - execute processes remotely
* [psfile](https://docs.microsoft.com/en-us/sysinternals/downloads/psfile) - shows files opened remotely
* psgetsid - display the sid of a computer or a user
* psinfo - list information about a system
* psping - measure network performance
* [pskill](https://docs.microsoft.com/en-us/sysinternals/downloads/pskill) - kill processes by name or process id
* [pslist](https://docs.microsoft.com/en-us/sysinternals/downloads/pslist) - list detailed information about processes
* psloggedon - see who's logged on locally and via resource sharing (full source is included)
* psloglist - dump event log records
* pspasswd - changes account passwords
* psservice - view and control services
* psshutdown - shuts down and optionally reboots a computer
* PsSuspend - suspends processes
### [process explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)
一个很强大的进程管理器, 可以把进程以树的形式展示出来.
![process explorer](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343728_20191226150020705_11738.png)
## 安全相关
### [sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
System Monitor (Sysmon) 是一个windows service. 安装后会输出详细的进程创建, 网络连接等信息.
## 系统信息
### [rammap](https://docs.microsoft.com/en-us/sysinternals/downloads/rammap)
可以利用此工具看到详细的不同内存的统计信息, 每个文件映射到内存中的size,乃至每一个物理内存对应哪些实际文件, 或者某文件在物理内存中的索引.
![rammap](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343724_20191225170051070_7770.png)
## 杂
### [testlimit](https://docs.microsoft.com/en-us/sysinternals/downloads/testlimit)
一个可以用来占据内存,handles, processes, threads等的工具. 需要搭配psexec使用, 例如: `.\PsExec.exe -sid Testlimit64.exe -r  1000 -c 1`
![testlimit](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343727_20191225174041767_2182.png)

reserve 1000 MB memory, reverse次数为1.
### [BlueScreen Screen Saver](https://docs.microsoft.com/en-us/sysinternals/downloads/bluescreen)
传统windows崩溃页面作为屏保.
### [zoom it](https://docs.microsoft.com/en-us/sysinternals/downloads/zoomit)
利用快捷键放大屏幕, 然后也可以做一些标注.
### [hex2dec](https://docs.microsoft.com/en-us/sysinternals/downloads/hex2dec)
十分方便的十进制与16进制转换,参看截图.

![hex2dec](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577343720_20191225164242847_4264.png)

> [呼呼呼山](http://code4fun.me)
> 2019-12-25 09:58:00
