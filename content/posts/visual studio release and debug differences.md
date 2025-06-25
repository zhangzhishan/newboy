---
title: visual studio release/debug differences
tag: cpp
date: 2019-04-30 16:41:38
---

在运行这个程序时，在debug模式下有问题，但是release模式下却可以。

注意查看。


```
#include <iostream>
#include <cstdio>

using namespace std;

int main(int argc, char *argv[])
{
	if (argc == 1) {
		cout << "Please specify the memory size in MB.\n";
		return 1;
	}

	long long sz = atoi(argv[1]);
	cout << "Allocating memory size: " << sz << "MB" << endl;

	if (sz <= 0) {
		cout << "Invalid memory size: " << sz << endl;
	}

	sz = sz * 1024 * 1024;
	cout << "size of buffer:" << sz << endl;
	char *buf = new char[sz];
	std::fill(buf, buf + sz, 1);

	cout << "Memory allocated. Press any key to exit." << endl;

	getchar();
	return 0;
}
```