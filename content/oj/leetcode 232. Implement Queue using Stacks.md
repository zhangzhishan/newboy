---
title: leetcode 232. Implement Queue using Stacks
tags: leetcode, oj
date: 25 Apr 2017 10:44 PM
---
	Implement the following operations of a queue using stacks.
	
	push(x) -- Push element x to the back of queue.
	pop() -- Removes the element from in front of queue.
	peek() -- Get the front element.
	empty() -- Return whether the queue is empty.


	class MyQueue {
	    stack<int> st1;
	    stack<int> st2;
	public:
	    /** Initialize your data structure here. */
	    MyQueue() {
	
	    }
	
	    /** Push element x to the back of queue. */
	    void push(int x) {
	        st1.push(x);
	    }
	
	    /** Removes the element from in front of queue and returns that element. */
	    int pop() {
	        int temp;
	        if(st2.empty()) {
	            while(!st1.empty()) {
	                temp = st1.top();
	                st1.pop();
	                st2.push(temp);
	            }
	
	        }
	        temp = st2.top();
	        st2.pop();
	        return temp;
	
	    }
	
	    /** Get the front element. */
	    int peek() {
	        int temp;
	        if(st2.empty()) {
	            while(!st1.empty()) {
	                temp = st1.top();
	                st1.pop();
	                st2.push(temp);
	            }
	
	        }
	        temp = st2.top();
	        return temp;
	    }
	
	    /** Returns whether the queue is empty. */
	    bool empty() {
	        if(st1.empty() && st2.empty()) {
	            return true;
	        }
	        else {
	            return false;
	        }
	    }
	};
	
	/**
	 * Your MyQueue object will be instantiated and called as such:
	 * MyQueue obj = new MyQueue();
	 * obj.push(x);
	 * int param_2 = obj.pop();
	 * int param_3 = obj.peek();
	 * bool param_4 = obj.empty();
	 */