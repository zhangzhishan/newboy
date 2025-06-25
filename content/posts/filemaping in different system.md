---
title: Filemap in windows and linux
tag: cpp
date: 2019-04-29 14:54:37
---
filemap的含义是将文件里面的一段内容映射到内存空间里面，从而可以提高读写速度。
## Windows
```cpp
// Now open view
HANDLE hFile = ::CreateFileA(p_fileName, GENERIC_READ, p_fileShareRead ? FILE_SHARE_READ : FILE_SHARE_DELETE,
                             0, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, 0);


if (INVALID_HANDLE_VALUE == hFile)
{
    LogAssert(INVALID_HANDLE_VALUE != hFile,
              "Cannot open file [%s] due to win32 error: %u\n",
              p_fileName,
              IndexServer::GetLastSystemError());
}

SCOPEGUARD(::MakeGuard(::CloseHandle, hFile));

HANDLE hMapping = ::CreateFileMapping(hFile, 0, PAGE_READONLY, 0, 0, 0);
LogAssert(0 != hMapping);
SCOPEGUARD(::MakeGuard(::CloseHandle, hMapping));

void* hView = FileUtils::MapViewOfFileAligned(hMapping,
                                          FILE_MAP_READ, 0, 0, 0);
```
上面为一个只读映射的file,`CreateFileA`参数描述:

* `GENERIC_READ` 允许对设备进行读访问
* `FILE_SHARE_READ` 允许对文件进行共享访问
* `OPEN_EXISTING` 文件必须存在
* `FILE_ATTRIBUTE_NORMAL` 默认属性

`CreateFileMapping`参数描述:

* `PAGE_READONLY` 只读属性, `hFile`需要对应为以`GENERIC_READ`形式打开
* `dwMaximumSizeHigh`，`dwMaximumSizeLow`：如果这两个参数为0，则文件映射对象的最大长度等于hFile指定的文件长度,需要注意的一点是当两个构成的区间大于现有的文件的时候，会对现在的文件做一个写操作，也即用0把文件扩充到指定的大小。
* `lpName` 此参数为空,创建一个无名字的文件映射对象.

`MapViewOfFile`参数描述:
```cpp
LPVOID MapViewOfFile(
  HANDLE hFileMappingObject,
  DWORD  dwDesiredAccess,
  DWORD  dwFileOffsetHigh,
  DWORD  dwFileOffsetLow,
  SIZE_T dwNumberOfBytesToMap
);
```

* `FILE_MAP_READ` ：  一个只读属性的文件视图被创建，保护模式为 `PAGE_READWRITE` 或 `PAGE_READONLY`
* The combination of the high and low offsets must specify an offset within the file mapping. They must also match the memory allocation granularity of the system. That is, the offset must be a multiple of the allocation granularity.
* `dwNumberOfBytesToMap`：映射文件部分的大小，如果为0，则映射整个文件。

可以利用`SYSTEM_INFO`的`dwAllocationGranularity`获取到虚拟内存的分配粒度，这个再linux下没有这个选项，一般取pagesize。

## linux

```cpp
int open(const char *pathname, int flags, mode_t mode);
```

The argument flags must include one of the following access modes: `O_RDONLY`, `O_WRONLY`, or `O_RDWR`.  These request opening the file read-only, write-only, or read/write, respectively.

```cpp
void* mmap(void *addr, size_t length, int prot, int flags,
                  int fd, off_t offset);
```

* If `addr` is `NULL`, then the kernel chooses the (page-aligned) address at which to create the mapping; this is the most portable method of creating a new mapping.
* The `length` argument specifies the length of the mapping (which must be greater than 0).
* The `prot` argument describes the desired memory protection of the mapping (and must not conflict with the open mode of the file).
* `offset` must be a multiple of the page size as returned by `sysconf(_SC_PAGE_SIZE)`.

在写操作上Linux和Windows有一个不同的点是，在linux下利用`mmap`获得文件指针之后，对指针内容进行修改之后必须利用`unmmap`操作，才能使得修改写到文件当中，可以查看下面的例子。
并且`mmap`的方式必须选择，`MAP_SHARED`。
当对一个文件做两次map操作的时候，如果map之后操作了`unmap`那么返回的地址可能是相同的，否则返回的是不同的地址。相当于unmap之后可以继续到指定的区域操作.
在把结构体写成二进制的时候,windows和linux的补齐位是不同的,一个是`CC`,一个是`00`.

Windows:

```cpp
#include<Windows.h>
#include <iostream>

int main()
{
	SYSTEM_INFO sysInfo;

	::GetSystemInfo(&sysInfo);
	auto gra = sysInfo.dwAllocationGranularity;
	//		gra	65536	unsigned long

	const char* ret = "test";
	const char* ret2 = "hekekdladfajgkjag";

	HANDLE hFile = ::CreateFileA("hellozhizha",
		GENERIC_WRITE | GENERIC_READ,
		FILE_SHARE_READ,
		0,
		OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL,
		0);

	HANDLE hMapFile = CreateFileMappingA(hFile,
		NULL,
		PAGE_READWRITE,
		0,
		1200000,
		0);
	// This can extend the file to offset size

    // 		hView	0x000001fd76800000	void *
	void* hView = ::MapViewOfFile(hMapFile,
		FILE_MAP_ALL_ACCESS,
		0,
		0,
		strlen(ret));

	memcpy(hView, ret, strlen(ret));

    //		checksumInFile	0x000001fd76970000	void *
	void* checksumInFile = ::MapViewOfFile(hMapFile,
		FILE_MAP_ALL_ACCESS,
		0,
		1 * gra,
		strlen(ret2));

	memcpy(checksumInFile, ret2, strlen(ret2));

	return 0;
}
```

Linux:

```cpp
#include <stdlib.h>
#include <sys/mman.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>


int main(void)
{
	const char* ret = "test";
	const char* ret2 = "hekekdladfajgkjag";
	auto gra = sysconf(_SC_PAGESIZE);

    // we need this file size larger than offset in advance.
	int fd = open("hellozhizha", O_RDWR, S_IRUSR | S_IWUSR | S_IRGRP | S_IROTH);

	void* hView = mmap(NULL, strlen(ret), PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
	memcpy(hView, ret, strlen(ret));
	munmap(hView, strlen(ret));

	void* checksumInFile = mmap(NULL, strlen(ret2), PROT_READ | PROT_WRITE, MAP_SHARED, fd, gra);
	memcpy(checksumInFile, ret2, strlen(ret2));
	munmap(checksumInFile, strlen(ret2));
	return 0;
}

```

> [呼呼呼山](http://code4fun.me)
> 2019-04-29 14:54:37
