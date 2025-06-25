---
title: leetcode 70. Climbing Stairs
tags: leetcode, oj
date: 2 May 2017 6:42 PM
---
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

本质上就是Fibonacci数列（想起了高中数学老师说的菲尔拨拉数列，哈哈哈）
	class Solution {
	public:
	    int climbStairs(int n) {
	        int ways[n + 1];
	        if(n >= 1) {
	            ways[1] = 1;
	        }
	        if (n >= 2) {
	            ways[2] = 2;
	        }
	        int k = 3;
	        while(k <= n) {
	            ways[k] = ways[k - 1] + ways[k - 2];
	            k++;
	        }
	        return ways[n];
	    }
	};

在leetcode上看到一种很有意思的解法。
	int climbStairs(int n) {
	    int a = 1, b = 1;
	    while (n--)
	        a = (b += a) - a;
	    return a;
	}

> [呼呼呼山]()(http://code4fun.me)
> 2 May 2017 6:42 PM 

