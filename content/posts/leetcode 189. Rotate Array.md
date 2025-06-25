---
title: leetcode 189. Rotate Array
tags: leetcode, oj
date: 25 Apr 2017 10:51 PM
---
	Rotate an array of n elements to the right by k steps.
	
	For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].


	class Solution {
	public:
	    void rotate(vector<int>& nums, int k) {
	        k = k % nums.size();
	        reverse(nums.begin(), nums.end());  
	    // 注意这里 vector 的 end()指向的是最后一个元素的下一个位置
	    // 而reverse 是一个左闭右开有始无终的操作
	        reverse(nums.begin(), nums.begin() + k);
	        reverse(nums.begin() + k, nums.end());
	    }
	};