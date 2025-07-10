---
title: leetcode 338. Counting Bits
tags: leetcode, oj
date: 2 May 2017 7:05 PM
---
	Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.
	
	Example:
	For num = 5 you should return [0,1,1,2,1,2].

	class Solution {
	public:
	    vector<int> countBits(int num) {
	        vector<int> result(num + 1, 0);
	        for(int i = 1; i <= num; i++) {
	            // 发现i的变化规律
	            result[i] = result[i & (i - 1)] + 1;
	        }
	        return result;
	    }
	};

> [呼呼呼山]()(http://code4fun.me)
> 2 May 2017 7:05 PM 

