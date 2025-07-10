---
title: leetcode 165. Compare Version Numbers
tags: leetcode, oj
date: 25 Apr 2017 10:15 PM
---
	Compare two version numbers version1 and version2.
	If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.
	
	You may assume that the version strings are non-empty and contain only digits and the . character.
	The . character does not represent a decimal point and is used to separate number sequences.
	For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.
	
	Here is an example of version numbers ordering:
	
	0.1 < 1.1 < 1.2 < 13.37


	class Solution {
	public:
	    int compareVersion(string version1, string version2) {
	    for(auto& w : version1) if (w == '.') w=' ';
	    for(auto& w : version2) if (w == '.') w=' ';
	    istringstream s1(version1), s2(version2);
	    while(1) {
	        int n1,n2;
	        if (not(s1 >> n1) ) n1 = 0;
	        if (not(s2 >> n2) ) n2 = 0;
	        if (not s1 and not s2) return 0;
	        if (n1<n2) return -1;
	        if (n1>n2) return 1;
	    }
	}
	};