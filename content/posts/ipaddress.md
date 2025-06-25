---
title: convert ip address from string to uint in windows
tag: cpp
date: 2019-03-26 09:36:55
---


```cpp
#include <windows.h>
#include <winsock2.h>
#include <ws2tcpip.h>
#include <iphlpapi.h>
#include <stdio.h>

#pragma comment(lib,"Ws2_32.lib")

using namespace std;

int main()
{
	struct sockaddr_in sa;
	char str[INET_ADDRSTRLEN];

	// store this IP address in sa:
	inet_pton(AF_INET, "25.125.211.145", &(sa.sin_addr));

	printf("%lu\n", sa.sin_addr); //2446556441
	printf("%d\n", sa.sin_addr); //-1848410855

	// now get it back and print it
	inet_ntop(AF_INET, &(sa.sin_addr), str, INET_ADDRSTRLEN);

	printf("%s\n", str); // prints "25.125.211.145"

	getchar();

	return 0;
}
```