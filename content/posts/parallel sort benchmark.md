---
title: Benchmark of parallel sort in boost and ppl
tag: cpp
date: 2019-04-19 17:50:41
---

VS 执行的时候的时间差距很大，不知道为甚？直接执行100000000个随机数排序的状态下，`boost`和`ppl`里面的差距在`0.5`秒以内。

```

#include <iostream>
#include <algorithm>
#include <random>
#include <cstdlib>
#include <vector>
#include <ppl.h>
#include <boost/sort/parallel_stable_sort/parallel_stable_sort.hpp>
#include <chrono>
#include <ctime>


int main()
{
	uint32_t NELEM = 100000000;
	std::mt19937 my_rand(0);
	std::vector< uint64_t > A, B;
	A.reserve(NELEM);

	for (uint32_t i = 0; i < NELEM; ++i)
	{
		A.push_back(my_rand());
		// std::cout << A[i] << std::endl;
	}

	auto start = std::chrono::system_clock::now();
	auto end = std::chrono::system_clock::now();
	auto elapsed_seconds = end - start;

	B = A;
	start = std::chrono::system_clock::now();
	boost::sort::parallel_stable_sort(B.begin(), B.end());
	end = std::chrono::system_clock::now();

	elapsed_seconds = end - start;
	std::cout << "boost::sort::parallel_stable_sort elapsed time: " << std::chrono::duration<double>(elapsed_seconds).count() << "s\n";

	B = A;
	start = std::chrono::system_clock::now();
	concurrency::parallel_sort(B.begin(), B.end());
	end = std::chrono::system_clock::now();

	elapsed_seconds = end - start;
	std::cout << "concurrency::parallel_sort elapsed time: " << std::chrono::duration<double>(elapsed_seconds).count() << "s\n";

	B = A;
	start = std::chrono::system_clock::now();
	std::sort(B.begin(), B.end());
	end = std::chrono::system_clock::now();

	elapsed_seconds = end - start;

	std::cout << "std::sort elapsed time: " << std::chrono::duration<double>(elapsed_seconds).count() << "s\n";
	return 0;

}
```

> [呼呼呼山]()(http://code4fun.me)
> 2019-04-19 17:50:41
