---
title: leetcode 168. Excel Sheet Column Title
tags: leetcode, oj
date: 25 Apr 2017 10:41 PM
---
	Given a positive integer, return its corresponding column title as appear in an Excel sheet.
	
	For example:
	
	    1 -> A
	    2 -> B
	    3 -> C
	    ...
	    26 -> Z
	    27 -> AA
	    28 -> AB 


	class Solution {
	public:
	    string convertToTitle(int n) {
	        string result = "";
	        int flag = 0;
	        n = n - 1;
	        while(n >= 0) {
	            result += 'A' + n % 26;
	            n /= 26;
	            n -= 1;
	        }
	
	        reverse(result.begin(), result.end());
	        return result; 
	    }
	};