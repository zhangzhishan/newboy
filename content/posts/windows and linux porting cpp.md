---
title: Porting windows cpp api to linux
tag: cpp
date: 2019-04-15 15:40:13
---

```
#ifdef PLATFORM_UNIX
   #define ZeroMemory(Destination,Length) memset((Destination),0,(Length))
#endif
```

windows CaptureStackBackTrace -> Linux backtrace

```
//to reserve virtual address space 
//equivalent of VirtualAlloc(NULL, size, MEM_RESERVE, PAGE_NOACCESS)
void* ptr = mmap(NULL, size, PROT_NONE, (MAP_PRIVATE | MAP_ANON), -1, 0);
msync(ptr, size, (MS_SYNC | MS_INVALIDATE));

//to free ALL virtual address space 
//equivalent of VirtualFree(addr, 0, MEM_RELEASE)
//where "size" is the size of the entire virtual address space and "addr" the starting address
msync(addr, size, MS_SYNC); 
munmap(addr, size);

//to allocate physical memory
//equivalent of VirtualAlloc(addr, size, MEM_COMMIT, PAGE_READWRITE)
void* ptr = mmap(addr, size, (PROT_READ | PROT_WRITE), (MAP_FIXED | MAP_SHARED | MAP_ANON), -1, 0);
msync(addr, size, (MS_SYNC | MS_INVALIDATE));

// instead of unmapping the address, we're just gonna trick 
// the TLB to mark this as a new mapped area which, due to 
// demand paging, will not be committed until used. VirtualFree(addr, size, MEM_DECOMMIT);

mmap(addr, size, PROT_NONE, MAP_FIXED|MAP_PRIVATE|MAP_ANON, -1, 0);
msync(addr, size, MS_SYNC|MS_INVALIDATE);
```
## system info
### c++ 11
```
//may return 0 when not able to detect
unsigned concurentThreadsSupported = std::thread::hardware_concurrency();
```
### win32
```
SYSTEM_INFO sysinfo;
GetSystemInfo(&sysinfo);
int numCPU = sysinfo.dwNumberOfProcessors;
printf("%s %d\n\n", "PageSize[Bytes] :", sysInfo.dwPageSize);

MEMORYSTATUSEX mem;
mem.dwLength = sizeof(mem);
LogAssert(GlobalMemoryStatusEx(&mem));
UInt32 totalPhysicalMemoryInMB = static_cast<UInt32>(mem.ullTotalPhys / c_BytesPerMegabyte);
```
### linux
```

int numCPU = sysconf(_SC_NPROCESSORS_ONLN);
long pagesize = sysconf(_SC_PAGE_SIZE);
```

[sysinfo](http://man7.org/linux/man-pages/man2/sysinfo.2.html)
```cpp
#include "sys/sysinfo.h"
struct sysinfo memInfo;

sysinfo (&memInfo);
long long totalVirtualMem = memInfo.totalram;
//Add other values in next statement to avoid int overflow on right hand side...
totalVirtualMem += memInfo.totalswap;
totalVirtualMem *= memInfo.mem_unit;

long long virtualMemUsed = memInfo.totalram - memInfo.freeram;
//Add other values in next statement to avoid int overflow on right hand side...
virtualMemUsed += memInfo.totalswap - memInfo.freeswap;
virtualMemUsed *= memInfo.mem_unit;

long long totalPhysMem = memInfo.totalram;
//Multiply in next statement to avoid int overflow on right hand side...
totalPhysMem *= memInfo.mem_unit;

long long physMemUsed = memInfo.totalram - memInfo.freeram;
//Multiply in next statement to avoid int overflow on right hand side...
physMemUsed *= memInfo.mem_unit;
```
## sleep
```
void Sleep(
  DWORD dwMilliseconds
);

std::this_thread::sleep_for(std::chrono::milliseconds(100));
```
## time
### win32
```
GetSystemTimeAsFileTime
LPFILETIME
FILETIME
```
### cross platform
[chrono](https://zh.cppreference.com/w/cpp/header/chrono)

[std::chrono::system_clock::now()](https://zh.cppreference.com/w/cpp/chrono/system_clock/now)

## file system
### win32
WIN32_FIND_DATAA FindFirstFileA FindNextFileA GetTempPathA
### cross platform
[boost::filesystem](https://www.boost.org/doc/libs/1_38_0/libs/filesystem/doc/index.htm)

[some example in SO](https://stackoverflow.com/questions/612097/how-can-i-get-the-list-of-files-in-a-directory-using-c-or-c)

[use boost regex as a mask](https://stackoverflow.com/questions/1257721/can-i-use-a-mask-to-iterate-files-in-a-directory-with-boost)

temp_directory_path

## _stdcall
```
// https://stackoverflow.com/questions/52713849/why-does-gcc-ignore-attribute-stdcall
#ifndef XRAND2_API
#ifndef PLATFORM_UNIX
#define XRAND2_API __stdcall
#else
#define XRAND2_API __attribute__((stdcall))
#endif


#ifdef PLATFORM_UNIX
    #define CDECAL_MODIFIER  __attribute__((cdecl))
#else
    #define CDECAL_MODIFIER __cdecl
#endif
```

一些windows下的type的定义，[type](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-dtyp/66996877-9dd4-477d-a811-30e6c1a5525d)
## thread
std::thread 可以变为linux或者win32自己的thread,设置优先级 [native_handle](https://zh.cppreference.com/w/cpp/thread/thread/native_handle)
## char util
```
strcpy_s -> strcpy

// but this will lead a problem, if the dest char array is not initilized, the result will be wrong.!
strncpy_s -> strncpy https://zh.cppreference.com/w/c/string/byte/strncpy _TRUNCATE is size of dest

_snprintf -> snprintf
sprintf_s -> snprintf
strncat_s -> strncat https://zh.cppreference.com/w/c/string/byte/strncat
strcat_s -> strcat https://zh.cppreference.com/w/c/string/byte/strcat
sscanf_s -> sscanf
StringCchPrintfA -> snprintf
printf UInt64 I64u
```
## atomic
* Interlockedxxx -> std::atomic
* InterlockedIncrement -> atomic_inc
* InterlockedDecrement -> atomic_dec
## CONDITION_VARIABLE
### win32
```
CONDITION_VARIABLE
WakeAllConditionVariable
InitializeConditionVariable
InitializeCriticalSectionAndSpinCount
WakeConditionVariable
```
### std
```
condition_variable
wait
wait_for
notify_one
notify_all
```
## mutex
### win32
```
CRITICAL_SECTION
EnterCriticalSection
LeaveCriticalSection
InitializeCriticalSectionAndSpinCount
SleepConditionVariableCS
```
### std
```
std::mutex
lock
unlock
```
> [呼呼呼山](http://code4fun.me)
> 2019-04-15 15:40:13
