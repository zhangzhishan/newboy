---
title: cpp set iter
tag: cpp
date: 2019-03-26 09:36:55
---

In this article we will discuss different ways to iterate over a set in C++.

Let’s create a set of strings i.e.

```
// Set of strings
std::set<std::string> setOfStr = {
							"jjj",
							"khj",
							"bca",
							"aaa",
							"ddd" };
```

Now let's iterate over it and print the contents on screen using different methods i.e.

## Iterating over a Set using Iterators

`set::begin()` returns an iterator pointing to the first element in set. Whereas, `set::end()` returns an iterator past the end of set.

Now to iterate a set in forward direction, we need to create an iterator and initialise it with `set::begin()`. So that it points to start of set and then we will keep on access and increment the iterator to next till `set::end()` is reached i.e.

```
// Creating a iterator pointing to start of set
std::set<std::string>::iterator it = setOfStr.begin();

// Iterate till the end of set
while (it != setOfStr.end())
{
	// Print the element
	std::cout << (*it) << ",";
	//Increment the iterator
	it++;
}
```

Here we iterated the set in forward direction. Now, let’s see how to iterate in reverse direction.

## Iterating a set in backward direction using reverse_iterator

`set::rbegin()` returns a reverse_iterator pointing to the last element of set. Whereas, `set::rend()` returns a reverse_iterator pointing to element before the first element.

Now to iterate a set in reverse direction, we need to create an reverse_iterator and initialise it with `set::rbegin()`. So that it points to the last element of set and then we will keep on access and increment the iterator to next till `set::rend()` is reached i.e. beginning of set.

```
// Creating a reverse iterator pointing to end of set i.e. rbegin
std::set<std::string>::reverse_iterator revIt = setOfStr.rbegin();

// Iterate till the start of set i.e. rend
while (revIt != setOfStr.rend())
{
	// Print the element
	std::cout << (*revIt) << ",";
	//Increment the iterator
	revIt++;
}
```

## Iterating over a set using Range base for loop

```
// Iterate over all elements of set
// using range based for loop
for (auto elem : setOfStr)
{
	std::cout << elem << ",";
}
```

## Iterating over a set using std::for_each and range based for loop

```
// Iterate over all elements using for_each
// and lambda function
std::for_each(setOfStr.begin(), setOfStr.end(), [](const std::string & str)
{
	std::cout<<str<<",";
});
```

Complete example is as follows,

```
#include <iostream>
#include <set>
#include <string>
#include <algorithm>
 
int main()
{
// Set of strings
	std::set<std::string> setOfStr =
	{ "jjj", "khj", "bca", "aaa", "ddd" };
 
	std::cout << "*** Iterating Set in Forward Direction using Iterators ***"
			<< std::endl;
 
// Creating a iterator pointing to start of set
	std::set<std::string>::iterator it = setOfStr.begin();
 
// Iterate till the end of set
	while (it != setOfStr.end())
	{
		// Print the element
		std::cout << (*it) << ",";
		//Increment the iterator
		it++;
	}
	std::cout << std::endl;
 
	std::cout << "*** Iterating Set in Reverse Direction using Iterators ***"
			<< std::endl;
 
// Creating a reverse iterator pointing to end of set i.e. rbegin
	std::set<std::string>::reverse_iterator revIt = setOfStr.rbegin();
 
// Iterate till the start of set i.e. rend
	while (revIt != setOfStr.rend())
	{
		// Print the element
		std::cout << (*revIt) << ",";
		//Increment the iterator
		revIt++;
	}
	std::cout << std::endl;
 
	std::cout << "*** Iterating Set using range based for loop ***"
			<< std::endl;
 
// Iterate over all elements of set
// using range based for loop
	for (auto elem : setOfStr)
	{
		std::cout << elem << ",";
	}
	std::cout << std::endl;
 
	std::cout << "*** Iterating Set using for_each algo & Lambda function ***"
			<< std::endl;
 
// Iterate over all elements using for_each
// and lambda function
	std::for_each(setOfStr.begin(), setOfStr.end(), [](const std::string & str)
	{
		std::cout<<str<<",";
	});
	std::cout << std::endl;
}
```

Output:

```
*** Iterating Set in Forward Direction using Iterators ***
aaa , bca , ddd , jjj , khj , 
*** Iterating Set in Reverse Direction using Iterators ***
khj , jjj , ddd , bca , aaa , 
*** Iterating Set using range based for loop ***
aaa , bca , ddd , jjj , khj , 
*** Iterating Set using for_each algo & Lambda function ***
aaa , bca , ddd , jjj , khj ,
```