---
title: leetcode 543 Diameter of Binary Tree
tags: leetcode, oj
date: 25 Apr 2017 11:14 PM
---
题目如下：
	Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.
	
	Example:
	Given a binary tree 
	          1
	         / \
	        2   3
	       / \     
	      4   5    
	Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
	
	Note: The length of path between two nodes is represented by the number of edges between them.
解决思路是：求出每一个点左右的最大深度，然后就可以计算出以该节点作为根节点的最大。
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
	    int result = 0;
	    int diameterOfBinaryTree(TreeNode* root) {
	        maxDeep(root);
	        return result;
	
	    }
	    int maxDeep(TreeNode* root) {
	        if(root == NULL) {
	            return -1;
	        }
	        else if(root->left == NULL && root->right == NULL) {
	            return 0;
	        }
	        int leftDepth = maxDeep(root->left);
	        int rightDepth = maxDeep(root->right);
	        result = max(leftDepth + rightDepth + 2, result);
	        return max(leftDepth, rightDepth) + 1;
	    }
	};