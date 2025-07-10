---
title: leetcode 21. Merge Two Sorted Lists
tags: leetcode, oj
date: 25 Apr 2017 10:30 PM
---
	Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.


	class Solution {
	public:
	    ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
	        ListNode dummy(INT_MIN);
	        ListNode *tail = &dummy;
	
	        while (l1 && l2) {
	            if (l1->val < l2->val) {
	                tail->next = l1;
	                l1 = l1->next;
	            } else {
	                tail->next = l2;
	                l2 = l2->next;
	            }
	            tail = tail->next;
	        }
	
	        tail->next = l1 ? l1 : l2;
	        return dummy.next;
	    }
	};