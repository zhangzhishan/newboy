---
title: leetcode 160-Intersection of Two Linked Lists
tags: leetcode, oj
date: 25 Apr 2017 10:36 PM
---
	Write a program to find the node at which the intersection of two singly linked lists begins.
	
	For example, the following two linked lists:
	
	A:          a1 → a2
	                   ↘
	                     c1 → c2 → c3
	                   ↗            
	B:     b1 → b2 → b3
	begin to intersect at node c1.


	/**
	 * Definition for singly-linked list.
	 * struct ListNode {
	 *     int val;
	 *     ListNode *next;
	 *     ListNode(int x) : val(x), next(NULL) {}
	 * };
	 */
	class Solution {
	public:
	    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
	        int len_A = 0, len_B = 0;
	        ListNode *p1, *p2;
	        p1 = headA;
	        p2 = headB;
	        if(!p1 || !p2) {
	            return NULL;
	        }
	        while(p1->next) {
	            p1 = p1->next;
	            len_A++;
	        }
	        while(p2->next) {
	            p2 = p2->next;
	            len_B++;
	        }
	        if(p1 != p2) {
	            return NULL;
	        }
	        else {
	            p1 = headA;
	            p2 = headB;
	            if(len_A < len_B) {
	                ListNode *temp;
	                temp = p1;
	                p1 = p2;
	                p2 = temp;
	            }
	            int temp = 0;
	            while(p1) {
	                if(temp == abs(len_A - len_B)) {
	                    break;
	                }
	                p1 = p1->next;
	                temp++;
	            }
	            while(p2) {
	                if(p1 == p2) {
	                    return p1;
	                }
	                else {
	                    p1 = p1->next;
	                    p2 = p2->next;
	                }
	            }
	        }
	        return NULL;
	
	    }
	};