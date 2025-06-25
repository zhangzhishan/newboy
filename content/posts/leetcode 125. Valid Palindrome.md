---
title: leetcode 125. Valid Palindrome
tags: leetcode, oj
date: 25 Apr 2017 10:49 PM
---
	Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
	
	For example,
	"A man, a plan, a canal: Panama" is a palindrome.
	"race a car" is not a palindrome.


	class Solution {
	public:
	    bool isPalindrome(string s) {
	        transform(s.begin(), s.end(), s.begin(), ::toupper); 
	        string b = s;
	        reverse(s.begin(), s.end());
	        int j = 0;
	        for(int i = 0; i < s.length(); i++) {
	            if(!(s[i] <= '9' && s[i] >= '0') && !(s[i] <= 'Z' && s[i] >= 'A')) {
	                continue;
	            }
	            while(!(b[j] <= '9' && b[j] >= '0') && !(b[j] <= 'Z' && b[j] >= 'A')) {
	                j++;
	            }
	            if(s[i] != b[j]) {
	                return false;
	            }
	            j++;
	
	        }
	        return true;
	    }
	};