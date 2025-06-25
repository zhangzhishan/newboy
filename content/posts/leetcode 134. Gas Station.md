---
title: leetcode 134. Gas Station
tags: leetcode, oj
date: 10 May 2017 2:19 PM
---
	There are N gas stations along a circular route, where the amount of gas at station i is gas[i].
	
	You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.
	
	Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

	class Solution {
	public:
	    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
	        int start = gas.size() - 1;
	        int end = 0;
	        int sum = 0;
	        sum = gas[start] - cost[start];
	        while(start > end) {
	            if(sum >= 0) {
	                sum += gas[end] - cost[end];
	                end++;
	
	            }
	            else {
	                start--;
	                sum += gas[start] - cost[start];
	            }
	        }
	        return sum >= 0 ? start : -1;
	        
	    }
	};

> [呼呼呼山]()(http://code4fun.me)
> 10 May 2017 2:19 PM 

