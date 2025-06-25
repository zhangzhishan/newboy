---
title: _mm_prefetch
tags:
  - hardware
date: 2020-04-26 11:59:18
---

对于 `_mm_prefetch` 这个函数在 [intel 官方页面](https://software.intel.com/sites/landingpage/IntrinsicsGuide/#text=_mm_prefetch), 只提到了他的作用是 "Fetch the line of data from memory that contains address p to a location in the cache heirarchy specified by the locality hint i."

其每次prefetch的时候是一个cache line 也就是 64 bytes.

但是并没有相关的hint的取值, 后来找到了一个相关介绍的[slides](https://software.intel.com/sites/default/files/article/326703/5.3-prefetching-on-mic-4.pdf). 里面有具体的一些用例.

It works at cache-line granularity1: you only need to issue one prefetch for each cache line: more is just a waste. That means that in general, you should try to unroll your loop enough so that you can issue only one prefetch per cache line. In the case of 16-byte `__m128` values, that means unroll at least by 4 (which you've done, so you are good there).

一些参数的定义如下:

```cpp
#define _MM_HINT_T0 1
#define _MM_HINT_T1 2
#define _MM_HINT_T2 3
#define _MM_HINT_NTA 0
#define _MM_HINT_ENTA 4
#define _MM_HINT_ET0 5
#define _MM_HINT_ET1 6
#define _MM_HINT_ET2 7
```

对于不同参数的意思:

Fetches the line of data from memory that contains the byte specified with the source operand to a location in the cache hierarchy specified by a locality hint:

• T0 (temporal data)—prefetch data into all levels of the cache hierarchy.
• T1 (temporal data with respect to first level cache misses)—prefetch data into level 2 cache and higher.
• T2 (temporal data with respect to second level cache misses)—prefetch data into level 3 cache and higher, or an implementation-specific choice.
• NTA (non-temporal data with respect to all cache levels)—prefetch data into non-temporal cache structure and into a location close to the processor, minimizing cache pollution.

对于循环中的 prefetch, 我们可以给通过 `#pragmaloop count (200)` 来给 complier some hints.
另外就是也可以指定需要prefetch的 distance. 可以以 `#pragma prefetch var:hint:distance` 这个形式加到code里.
hint value can be in the range 0-3, distance in terms of iterations, 其中hint-0 means vprefetch0, hint-1 means vprefetch1, ..
一个示例如下:

```cpp
#pragma prefetchhtab_p:1:16
#pragma prefetchhtab_p:0:6
// Issue vprefetch1 for htab_pwith a distance of 16 vectorizediterations ahead
// Issue vprefetch0 for htab_pwith a distance of 6 vectorizediterations ahead
// If pragmas are not present, compiler chooses both distance values
```

vprefetch1 是指 first-level distance and vprefetch0 指 second-level prefetch distance.

n1 specifies the distance for first-level prefetches into L2.
n2 specifies prefetchdistance for second-level prefetches from L2 to L1 (use n2 <= n1)



> [呼呼呼山](http://code4fun.me)
> 2020-04-26 11:59:18
