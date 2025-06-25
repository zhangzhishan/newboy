---
title: leetcode 524. Longest Word in Dictionary through Deleting
tags: leetcode, oj
date: 25 Apr 2017 11:16 PM
---
	class Solution {
	public:
	    int max_len = 0;
	    string result;
	    // 对数组中的每个string都去判断是否可以由原string得到，过程中保存最长的字符串
	    string findLongestWord(string s, vector<string>& d) {
	        for(vector<string>::iterator p = d.begin(); p != d.end(); p++) {
	            findMax(s, *p);
	        }
	        return result;
	    }
	    void findMax(string s, string dic) {
	        int first;
	        for(int i = 0; i < dic.length();i++) {
	            first = s.find(dic[i]);
	            if(first == -1) {
	                return;
	            }
	            s.erase(0, first + 1);
	        }
	        if(max_len < dic.length()) {
	            max_len = dic.length();
	            result = dic;
	        }
	        else if(max_len == dic.length()) {
	            if(result > dic)
	                result = dic;
	        }
	    }
	};