---
title: Test of boost string performance
tag: cpp
date: 2019-03-18 17:30:43
---

```
#include <iostream>
#include <string>
#include <chrono>
#include <ctime>

#include <boost/algorithm/string.hpp>
#include <vector>

using std::string;
using std::vector;

void
split(const string& input, const string& delim, vector<string>& tokens)
{
    if (!delim.empty())
    {
        tokens.clear();
        size_t start = 0;
        size_t end = 0;
        while (end != string::npos)
        {
            end = input.find(delim, start);
            if (end >= start)
            {
                tokens.push_back(input.substr(start, end - start));
            }
            // Advance start past the delim.
            start = end + delim.size();
        }
    }
}

int main()
{
    auto start = std::chrono::system_clock::now();
    for (int i = 0; i < 10000; i++)
    {
        string tests = "a,v,v,v,x,sd,asf,e,r,afds,g";
        vector<string> ret;
        split(tests, ",", ret);
    }

    auto end = std::chrono::system_clock::now();

    std::chrono::duration<double> elapsed_seconds = end - start;

    std::cout << "basicstring elapsed time: " << std::chrono::duration<double>(elapsed_seconds).count() << "s\n";

    start = std::chrono::system_clock::now();
    for (int i = 0; i < 10000; i++)
    {
        string tests = "a,v,v,v,x,sd,asf,e,r,afds,g";
        vector<string> ret;
        boost::split(ret, tests, boost::is_any_of(","));
    }

    end = std::chrono::system_clock::now();

    elapsed_seconds = end - start;

    std::cout << "boost:split elapsed time: " << std::chrono::duration<double>(elapsed_seconds).count() << "s\n";
    return 0;
}
```




> [呼呼呼山]()(http://code4fun.me)
> 2019-03-18 17:30:43
