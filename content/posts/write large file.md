---
title: write large file
tag: cpp
date: 2019-04-18 15:54:35
---

```
// writefiletest.cpp : This file contains the 'main' function. Program execution begins and ends there.
//

#include "pch.h"
#include <iostream>
#include <Windows.h>
#include <vector>

typedef unsigned __int64 UInt64;
typedef unsigned __int32 UInt32;

bool
WriteLargeFile(HANDLE p_handle, LPCVOID p_buffer, UInt64 p_numberOfBytes, UInt32 p_maxLengthPerWrite = 1024 * 1024 * 1024UL)
{
    UInt64 bytesLeft = p_numberOfBytes;
    UInt64 totalBytesWritten = 0;
    DWORD bytesWritten = 0;

    while (bytesLeft > p_maxLengthPerWrite)
    {
        if (!WriteFile(p_handle, reinterpret_cast<const char*>(p_buffer) + totalBytesWritten, p_maxLengthPerWrite, &bytesWritten, nullptr)
            || (p_maxLengthPerWrite != bytesWritten))
        {

            return false;
        }
        totalBytesWritten += bytesWritten;
        bytesLeft -= bytesWritten;
    }

    if (bytesLeft > 0)
    {
        if (!WriteFile(p_handle, reinterpret_cast<const char*>(p_buffer) + totalBytesWritten, static_cast<UInt32>(bytesLeft), &bytesWritten, nullptr)
            || (bytesLeft != bytesWritten))
        {

            return false;
        }
    }

    return true;
}

int main()
{
    std::vector<UInt64> m_docIdsToDelete;
	m_docIdsToDelete.push_back(2);
	m_docIdsToDelete.push_back(5);
	m_docIdsToDelete.push_back(6);
	m_docIdsToDelete.push_back(3);

    std::unique_ptr<std::remove_pointer<HANDLE>::type, decltype(&::CloseHandle)> debugFileHandle(nullptr, &::CloseHandle);
    std::string s = "zhzihats";
    debugFileHandle.reset(::CreateFileA(s.c_str(),
        GENERIC_WRITE,
        NULL,
        nullptr,
        CREATE_ALWAYS,
        FILE_ATTRIBUTE_NORMAL,
        nullptr));
    std::cout << "Hello World!\n"; 
    WriteLargeFile(debugFileHandle.get(), reinterpret_cast<char*>(&((m_docIdsToDelete)[0])), sizeof(UInt64) * m_docIdsToDelete.size());
}
```

```
#include <fstream>
#include <iterator>
#include <string>
#include <vector>

typedef uint64_t UInt64;
typedef uint32_t UInt32;
typedef void * LPCVOID;


bool
WriteLargeFile(std::ofstream& fileHandle, LPCVOID p_buffer, UInt64 p_numberOfBytes, UInt32 p_maxLengthPerWrite=3)
{
    UInt64 bytesLeft = p_numberOfBytes;
    UInt64 totalBytesWritten = 0;

    while (bytesLeft > p_maxLengthPerWrite)
    {
        fileHandle.write(reinterpret_cast<const char*>(p_buffer) + totalBytesWritten, p_maxLengthPerWrite);
        if (!fileHandle.good())
        {
            return false;
        }
        totalBytesWritten += p_maxLengthPerWrite;
        bytesLeft -= p_maxLengthPerWrite;
    }

    if (bytesLeft > 0)
    {
        fileHandle.write(reinterpret_cast<const char*>(p_buffer) + totalBytesWritten, static_cast<UInt32>(bytesLeft));
        if (!fileHandle.good())
        {
            return false;
        }
    }

    return true;
}

int main()
{
    std::vector<UInt64> m_docIdsToDelete;
    m_docIdsToDelete.push_back(2);
    m_docIdsToDelete.push_back(5);
    m_docIdsToDelete.push_back(6);
    m_docIdsToDelete.push_back(3);
    std::string s = "zhzihats";

    std::ofstream debugFile(s.c_str(), std::ios::out | std::ios::binary);
    WriteLargeFile(debugFile, reinterpret_cast<char*>(&((m_docIdsToDelete)[0])), sizeof(UInt64) * m_docIdsToDelete.size());
}
```


> [呼呼呼山]()(http://code4fun.me)
> 2019-04-18 15:54:35
