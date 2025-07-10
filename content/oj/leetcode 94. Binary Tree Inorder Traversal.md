---
title: leetcode 94. Binary Tree Inorder Traversal
tags: leetcode, oj
date: 25 Apr 2017 10:17 PM
---
	Given a binary tree, return the inorder traversal of its nodes' values.
	
	For example:
	Given binary tree [1,null,2,3],
	   1
	    \
	     2
	    /
	   3
	return [1,3,2].
	


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
	private:
	    vector<int> inorderTraversal(TreeNode* root, vector<int> before) {
	        if(!root)   return before;
	        before = inorderTraversal(root->left, before);
	        before.push_back(root->val);
	
	        before = inorderTraversal(root->right, before);
	        return before;
	    }
	public:
	    vector<int> inorderTraversal(TreeNode* root) {
	        vector<int> result;
	        return inorderTraversal(root, result);
	    }
	};