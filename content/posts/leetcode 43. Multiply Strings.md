---
title: leetcode 43. Multiply Strings
tags: leetcode, oj
date: 25 Apr 2017 10:52 PM
---
	Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2.
	
	


	class Solution {
	public:
	    string multiply(string num1, string num2) {
	        string s;
	        int maxnum = 222;
	        int result[maxnum] = {0};
	        bool flag = true;
	        for(int i = 0; i < num1.length(); i++) {
	            for(int j = 0; j < num2.length(); j++) {
	                result[i + j] += (num1[num1.length() - 1 - i] - '0') * (num2[num2.length() - 1 -j] - '0') % 10;
	                result[i + j + 1] += (num1[num1.length() - 1 - i] - '0') * (num2[num2.length() - 1 -j] - '0') / 10;
	                if(result[i + j] >= 10) {
	                    result[i + j + 1] += 1;
	                    result[i + j] -= 10;
	                }
	                if(result[i + j + 1] >= 10) {
	                    result[i + j + 2] += 1;
	                    result[i + j + 1] -= 10;
	                }
	            }
	        }
	        for(int i = maxnum - 1; i >= 0; i--) {
	            if(flag && result[i] == 0) {
	                continue;
	            }
	            flag = false;
	
	            s += to_string(result[i]);
	
	        }
	        if(flag) s = '0';
	
	        return s;
	
	    }
	};