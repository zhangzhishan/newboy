---
title: leetcode 96. Unique Binary Search Trees
tags: leetcode, oj
date: 2 May 2017 7:32 PM
---
	Given n, how many structurally unique BST's (binary search trees) that store values 1...n?
	
	For example,
	Given n = 3, there are a total of 5 unique BST's.
	
	   1         3     3      2      1
	    \       /     /      / \      \
	     3     2     1      1   3      2
	    /     /       \                 \
	   2     1         2                 3
	

	class Solution {
	public:
	    int numTrees(int n) {
	        int dp[n + 1] = {0};
	        dp[0] = 1;
	        dp[1] = 1;
	        for(int i = 2; i <= n; i++) {
	            for(int j = 1; j <= i; j++) {
	                dp[i] += dp[j - 1] * dp[i - j];
	            }
	        }
	        
	        return dp[n];
	    }
	};

可以根据左右对称性对问题做进一步优化。

> [呼呼呼山]()(http://code4fun.me)
> 2 May 2017 7:32 PM 

