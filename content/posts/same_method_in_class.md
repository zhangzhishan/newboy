---
title: 如果一个类同时继承的两个类都定义了某一个函数会怎样呢
tag: cpp
date: 2019-05-24 15:40:15
---

入下面所示：类`C`同时继承了`A`和`B`,并且两个都定义了相同的函数,如果这时候编译的话会得到错误.

```
testhir.cpp: In function ‘int main(int, const char**)’:
testhir.cpp:26:7: error: request for member ‘printR’ is ambiguous
     c.printR();
       ^~~~~~
testhir.cpp:5:14: note: candidates are: void A::printR()
         void printR()
              ^~~~~~
testhir.cpp:13:14: note:                 void B::printR()
         void printR()
              ^~~~~~
```

```
#include <iostream>

class A {
    public:
        void printR()
        {
            std::cout << "in A" << std::endl;
        }
};

class B {
    public:
        void printR()
        {
            std::cout << "in B" << std::endl;
        }
};

class C :B,A {
public:

};

int main(int argc, const char** argv) {
    C c;
    c.printR();
    return 0;
}
```

如果在C中也实现了相同的函数的话,就可以正常编译获得结果:`in C`

```
#include <iostream>

class A {
    public:
        void printR()
        {
            std::cout << "in A" << std::endl;
        }
};

class B {
    public:
        void printR()
        {
            std::cout << "in B" << std::endl;
        }
};

class C :B,A {
public:
void printR()
        {
            std::cout << "in C" << std::endl;
        }
};

int main(int argc, const char** argv) {
    C c;
    c.printR();
    return 0;
}
```

> [呼呼呼山](http://code4fun.me)
> 2019-05-24 15:40:15
