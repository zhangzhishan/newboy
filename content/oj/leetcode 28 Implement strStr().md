---
title: leetcode 28 Implement strStr()
tags: leetcode, oj
date: 25 Apr 2017 10:42 PM
---
	Total Accepted: 173158
	Total Submissions: 628656
	Difficulty: Easy
	Contributor: LeetCode
	Implement strStr().
	
	Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.


	class Solution {
	public:
	    int strStr(string haystack, string needle) {
	        if(haystack.length() < needle.length()) {
	            return -1;
	        }
	        for(int i = 0; i <= haystack.length() - needle.length(); i++) {
	            bool flag = true;
	            for(int j = 0; j < needle.length(); j++)
	                if(haystack[i + j] != needle[j]) {
	                    flag = false;
	                    break;
	                }
	            if(flag)
	                return i;
	        }
	        return -1;
	    }
	};