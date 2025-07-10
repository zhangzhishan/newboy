---
title: 103. Binary Tree Zigzag Level Order Traversal
tags: oj, leetcode
date: 25 Apr 2017 10:13 PM
---
题目要求：
	Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
	
	For example:
	Given binary tree [3,9,20,null,null,15,7],
	    3
	   / \
	  9  20
	    /  \
	   15   7
	return its zigzag level order traversal as:
	[
	  [3],
	  [20,9],
	  [15,7]
	]


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
	    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
	        queue<TreeNode*> node_queue;
	        queue<TreeNode*> node_queue_temp;
	        node_queue.push(root);
	        vector<vector<int>> result;
	        vector<int> temp_result;
	        bool direction = true;
	        while(!node_queue.empty()) {
	            TreeNode* now_node = node_queue.front();
	            node_queue.pop();
	            if(now_node != NULL) {
	                temp_result.push_back(now_node->val);
	                node_queue_temp.push(now_node->left);
	                node_queue_temp.push(now_node->right);
	            }
	            if(node_queue.empty() && temp_result.size() > 0) {
	                if(direction)
	                    result.push_back(temp_result);
	                else {
	                    reverse(temp_result.begin(), temp_result.end());
	                    result.push_back(temp_result);
	                }
	
	                direction = !direction;
	                temp_result.clear();
	                queue<TreeNode*> empty;
	                node_queue = node_queue_temp;
	                swap(node_queue_temp, empty );
	
	            }
	        }
	        return result;
	    }
	};