---
title: leetcode 100 same tree
tags: leetcode, oj
date: 25 Apr 2017 11:17 PM
---

Given two binary trees, write a function to check if they are equal or not.
Two binary trees are considered equal if they are structurally identical and the nodes have the same value.


	/**
	 * Definition for a binary tree node.
	 * struct TreeNode {
	 *     int val;
	 *     TreeNode *left;
	 *     TreeNode *right;
	 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
	 * };
	 */
	class Solution {
	public:
	    bool isSameTree(TreeNode* p, TreeNode* q) {
	        if(p == NULL && q == NULL) {
	            return true;
	        }
	        else if(p == NULL || q == NULL || p->val != q->val) {
	            return false;
	        }
	        else {
	            return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
	        }
	    }
	};