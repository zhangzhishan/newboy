---
title: leetcode 345. Reverse Vowels of a String
tags: leetcode, oj
date: 26 Apr 2017 10:51 PM
---
Write a function that takes a string as input and reverse only the vowels of a string.
Example 1:  
Given s = "hello", return "holle".
Example 2:  
Given s = "leetcode", return "leotcede".
	class Solution {
	public:
	    unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
	    string reverseVowels(string s) {
	        
	        int start = 0;
	        int end = s.length() - 1;
	        while(start < end) {
	            if(vowels.find(s[start]) != vowels.end() && vowels.find(s[end]) != vowels.end()) {
	                swap(s[start], s[end]);
	                start++;
	                end--;
	            }
	            else if(vowels.find(s[start]) != vowels.end()) {
	                end--;
	            }
	            else if(vowels.find(s[end]) != vowels.end()) {
	                start++;
	            }
	            else {
	                start++;
	                end--;
	            }
	        }
	        return s;
	    }
	};
