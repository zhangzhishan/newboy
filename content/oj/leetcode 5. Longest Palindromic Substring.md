---
title: leetcode 5. Longest Palindromic Substring
tags: leetcode, oj
date: 25 Apr 2017 10:53 PM
---
	Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
	
	Example:
	
	Input: "babad"
	
	Output: "bab"
	
	Note: "aba" is also a valid answer.
	Example:
	
	Input: "cbbd"
	
	Output: "bb"


	class Solution {
	public:
	    string longestPalindrome(string s) {
	        string result_s;
	        int test;
	        bool dp[s.length()][s.length()];
	
	        int result = 0;
	        for(int i = 0; i < s.length();i++) {
	            dp[i][i] = 1;
	            result = 1;
	            result_s = s[0];
	
	        }
	        for(int i = s.length() - 1; i >= 0; i--) {
	            for(int j = i; j < s.length();j++) {
	                dp[i][j] = (s[i] == s[j]) // chars at i and j should match
	                           && 
	                           ( j-i < 3  // if window is less than or equal to 3, just end chars should match
	                             || dp[i+1][j-1]  ); 
	                if(dp[i][j] && j - i + 1 > result) {
	                    result = j - i + 1;
	                    result_s = s.substr(i, result);
	                }
	
	            }
	        }
	        return result_s;
	
	    }
	};

