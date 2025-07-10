---
title: leetcode Two Sum
tags: leetcode, oj
date: 1 May 2017 7:51 PM
---
	Given an array of integers, return indices of the two numbers such that they add up to a specific target.
	
	You may assume that each input would have exactly one solution, and you may not use the same element twice.
	
	Example:
	Given nums = [2, 7, 11, 15], target = 9,
	
	Because nums[0] + nums[1] = 2 + 7 = 9,
	return [0, 1].
	

	class Solution {
	public:
	    vector<int> twoSum(vector<int>& numbers, int target) {
	        unordered_map<int, int> hash;
		    vector<int> result;
		    for (int i = 0; i < numbers.size(); i++) {
			    int numberToFind = target - numbers[i];
	
	            //if numberToFind is found in map, return them
			    if (hash.find(numberToFind) != hash.end()) {
				    result.push_back(hash[numberToFind]);
				    result.push_back(i);			
				    return result;
			    }
	
	            //number was not found. Put it in the map.
			    hash[numbers[i]] = i;
		    }
		    return result;
	    }
	};

> [呼呼呼山]()(http://code4fun.me)
> 1 May 2017 7:51 PM 

