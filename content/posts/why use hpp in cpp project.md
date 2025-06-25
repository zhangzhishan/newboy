---
title: why use hpp in cpp project
tag: cpp
date: 2019-03-26 09:36:55
---


Here are a couple of reasons for having different naming of C vs C++ headers:

* Automatic code formatting, you might have different guidelines for formatting C and C++ code. If the headers are separated by extension you can set your editor to apply the appropriate formatting automatically
* Naming, I've been on projects where there were libraries written in C and then wrappers had been implemented in C++. Since the headers usually had similar names, i.e. Feature.h vs Feature.hpp, they were easy to tell apart.
* Inclusion, maybe your project has more appropriate versions available written in C++ but you are using the C version (see above point). If headers are named after the language they are implemented in you can easily spot all the C-headers and check for C++ versions.

Remember, C is not C++ and it can be very dangerous to mix and match unless you know what you are doing. Naming your sources appropriately helps you tell the languages apart.