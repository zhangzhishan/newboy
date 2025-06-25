---
title: SPSCQueue 介绍
tag: cpp
date: 2019-03-26 09:36:55
---

## SPSCQueue
A fast single-producer, single-consumer lock-free queue for C++, [boost library](https://www.boost.org/doc/libs/1_53_0/doc/html/boost/lockfree/spsc_queue.html)

无锁队列之单生产者单消费者

现实应用场景中经常会用到单生产者单消费者或多生产者多消费者模型，例如一个写线程，接收到数据写入缓冲区，另一个线程读出并处理。为了尽可能减少锁所占用的时间，可使用gcc的一些原子操作来代替pthread_mutex_t或pthread_spinlock_t。

核心思想：预分配一段连续的内存，用min_seq_表示读的位置，max_seq_表示写的位置，当写满或者读完时返回错误；如果不同步有可能发生的错误是：读线程读取max_seq_时，刚好写线程在修改max_seq_，min_seq_同上，故使用__sync_add_and_fetch 函数自增0或者1来读取或者修改变量。

注意事项：分配的内存大小一定要足够大，否则可能会造成数据的丢失。 
就好比一件仓库，有人不停往里存放东西，有人不停往外取，当仓库较小时，空间满时，可能会出现存放失败，那么这件商品就丢失了。


```cpp
#pragma once
#include <windows.h>
#include <boost/utility.hpp>
#include "basic_types.h"

// One lock free queue with a great performance with following limitations:
// 1: Only support one consumer and one producer at most.
// 2: Put/Get times can't exceed MAX_UINT64.
// 3: Read/write of m_head/m_tail is atomic (single instruction).

template<typename T>
class SPSCQueue : private boost::noncopyable
{
public:
    SPSCQueue() : m_ptrArray(NULL), m_maxQueueLength(0)
    {
    }

    SPSCQueue(UInt64 p_maxQueueLength) : m_ptrArray(NULL)
    {
        Reset(p_maxQueueLength);
    }

    // Reset max queue length.
    void Reset(UInt64 p_maxQueueLength)
    {
        LogAssert (p_maxQueueLength > 0);
        delete []m_ptrArray;
        m_maxQueueLength = p_maxQueueLength;
        m_ptrArray = new T*[m_maxQueueLength];
        m_head = m_tail = 0;
    }

    ~SPSCQueue()
    {
        LogAssert(Empty());
        delete []m_ptrArray;
    }

    // Add new object to queue's tail.
    void PutWait(T *p_object)
    {
        int backoff = 0;
        while (m_tail - m_head >= m_maxQueueLength)
        {
            SpinPauseWithBackoff (backoff);
        }

        m_ptrArray[m_tail % m_maxQueueLength] = p_object;
        ++m_tail;
    }

    // Get object from queue's head.
    T* GetWait()
    {
        int backoff = 0;
        while (m_tail == m_head)
        {
            SpinPauseWithBackoff (backoff);
        }

        T* ret = m_ptrArray[m_head % m_maxQueueLength];
        ++m_head;
        return ret;
    }

    // Get object from queue's head, caller need to make sure queue is not empty,
    // otherwise the result is not defined.
    T* Get()
    {
        T* ret = m_ptrArray[m_head % m_maxQueueLength];
        ++m_head;
        return ret;
    }

    T* Peek()
    {
        return m_ptrArray[m_head % m_maxQueueLength];
    }

    // Peek object from queue's head.
    T* PeekWait()
    {
        int backoff = 0;
        while (m_tail == m_head)
        {
            SpinPauseWithBackoff (backoff);
        }
        return m_ptrArray[m_head % m_maxQueueLength];
    }

    UInt64 Size() const
    {
        return m_tail - m_head;
    }

    bool Empty() const
    {
        return m_head == m_tail;
    }

private:
    static void SpinPauseWithBackoff(int& p_backoff)
    {
        int yieldCount = p_backoff;
        do
        {
            YieldProcessor ();
        }
        while (--yieldCount >= 0);
        // Backoff is always power of 2 minus 1.
        if (p_backoff <= 511)
        {
            p_backoff = p_backoff * 2 + 1;
        }
    }

    // Buffer array.
    T** m_ptrArray;

    // Max Queue length.
    UInt64 m_maxQueueLength;

    // Queue's head index.
    __declspec(align(64)) volatile UInt64 m_head;

    // Queue's tail index.
    __declspec(align(64)) volatile UInt64 m_tail;
};
```