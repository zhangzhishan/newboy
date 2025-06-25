---
title: windbg
tag: debug
date: 2019-04-30 16:40:28
---
A very good documents: [Common WinDbg Commands (Thematically Grouped)](http://windbg.info/doc/1-common-cmds.html)
## symbols related.
* `.sympath srv*https://msdl.microsoft.com/download/symbols` Add msdl path to symbol path.
* `!sym noisy ` we can get extra symbol info in Windbg.
* `.reload /f ntdll.dll` reload the symbol file for given file.

## breakpoint
[reference](https://www.cnblogs.com/renyuan/p/6206309.html)

```
bm Saas1!Saas::ISRCreatorDocShard::~ISRCreatorDocShard
bp `Saas1!l01rankingcontext.cpp:103`
```

### conditional breakpoint

```
bu MidLevelAggregator!MLAQueryAggregateProcessor::FinaliseMetaData ".if(((@@c++(totalData->m_nNodesQueried))>0n0)){ .echo \"hit\"} .else{gc}"

bu MidLevelAggregator!MLAQueryAggregateProcessor::FinaliseMetaData ".if(((@@c++(sizeof(totalData->m_indexVersions)))>0x28)){ .echo \"hit\"} .else{gc}"

bu Saas2!AggregatorPacketServer::ProcessReceive ".if(((@@c++(request->m_ClientAddr.sin_addr.S_un.S_addr))==0xaecc7d19 | request->m_ClientAddr.sin_addr.S_un.S_addr))==0xb1cc7d19)){ .echo \"hit\"} .else{gc}"
```
notice the space before `.`
## variables
dx requestMachine->m_child.m_machine.m_impl.m_machine

dx child[3]->m_child.m_machine.m_impl.m_machine

dv：显示局部变量

## Stack

- `kb` - stack trace (current thread)
- `kp` - this will not show the information of Args to child
- `kP` -Args to child are shown in the symbol model)
- `.frame #` - change the current frame #

## Threads

- `~` - List all threads
- `~ kb` - List all threads and their stack
- `~# s` - set the current thread to # (the thread number)

## show C++ results
we have the C++ expression parser - `@@c++()` - that supports all forms of C++ expression syntax, such as:

* Numbers in C++ expressions
* Characters and strings in C++ expressions
* Symbols in C++ expressions. (see WinDbg documentation for details)
* Operators in C++ expressions
* Registers and pseudo-registers in C++ expressions
* Macros in C++ expressions

```
?? @@c++(sizeof(this->m_rec))
```
## memory related
* `!address -summary`
* `!address -f Heap`
* `!heap -i HeapAddress`
## other
* `.logopen (Open Log File) ` save windbg command output to given logfile.