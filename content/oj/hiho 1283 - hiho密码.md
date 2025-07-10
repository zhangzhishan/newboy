---
title: hiho 1283 - hiho密码
tags: oj
date: 25 Apr 2017 11:16 PM
---

```cpp
#include<stdio.h>

int main()
{
    int N;
    scanf("%d", &N);
    int x[N];
    int mid = 0;
    for(int i = 0;i < N;i++){
        scanf("%d", &x[i]);
    }
    for(int i = N - 1;i > 0;i--) {
        if(x[i] < x[i - 1]) {
            mid = i;
            break;
        }
    }
    for(int j = 0; j < mid;j++) {
        printf("%d", x[j]);
        if (j < mid - 1) {
            printf(" ");
        }

    }
    if(mid == 0) {
        printf("%d", x[0]);
    }
}
```
