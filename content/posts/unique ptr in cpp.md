---
title: unique_ptr usage
tag: cpp
date: 2019-03-26 09:36:55
---

## unique ptr in cpp
`std::unique_ptr` is used to replace `auto_ptr`.

When you say `std::unique_ptr<T>`, the `unique_ptr` constructor expects to receive a `T*`, but `CreateMutex` returns a `HANDLE`, not a `HANDLE *`.

we can use `std::remove_pointer` to get to the `HANDLE`'s underlying type.

When use it in windows API and deal with `HANDLE`, we need a different method to construct it using lambda delter and `decltype` to deduce the type of the function pointer.

```
std::unique_ptr<std::remove_pointer<HANDLE>::type, 
                decltype(&::CloseHandle)> m_mutex(::CreateMutex(NULL, FALSE, NULL),
                                                  &::CloseHandle);
```