Single Producer Single Consumer Queue
只支持单读单写的queue，把数据以 buffer array的形式存储，这个设计问题感觉还是挺多的，需要使用的时候十分小心，否则可能遇到挺多坑的。

windows 下的 `YieldProcessor`,  Signals to the processor to give resources to threads that are waiting for them. This macro is only effective on processors that support technology allowing multiple threads running on a single processor, such as Intel's Hyperthreading technology.

In computer science, yield is an action that occurs in a computer program during multithreading, of forcing a processor to relinquish control of the current running thread, and sending it to the end of the running queue, of the same scheduling priority. 

back off 让开
从0开始，每次backoff 的次数都是`(prev-1)*2+1`， 也即2^k+1, 最大值为512次。

```
#pragma once
#include <thread>
#include <boost/utility.hpp>
#include "basic_types.h"
#include "logging.h"

// One lock free queue with a great performance with following limitations:
// 1: Only support one consumer and one producer at most.
// 2: Put/Get times can't exceed MAX_UINT64.
// 3: Read/write of m_head/m_tail is atomic (single instruction).

template<typename T>
class SPSCQueue : private boost::noncopyable
{
public:
    SPSCQueue() : m_ptrArray(nullptr), m_maxQueueLength(0)
    {
    }


    SPSCQueue(UInt64 p_maxQueueLength) : m_ptrArray(nullptr)
    {
        Reset(p_maxQueueLength);
    }


    // Reset max queue length.
    void Reset(UInt64 p_maxQueueLength)
    {
        LogAssert(p_maxQueueLength > 0);
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
            SpinPauseWithBackoff(backoff);
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
            SpinPauseWithBackoff(backoff);
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
            SpinPauseWithBackoff(backoff);
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
            std::this_thread::yield();
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
    alignas(64) volatile UInt64 m_head;

    // Queue's tail index.
    alignas(64) volatile UInt64 m_tail;
};
```