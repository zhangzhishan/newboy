---
title: leetcode 102. Binary Tree Level Order Traversal
tags: leetcode, oj
date: 25 Apr 2017 10:46 PM
---
	Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).
	
	For example:
	Given binary tree [3,9,20,null,null,15,7],
	    3
	   / \
	  9  20
	    /  \
	   15   7
	return its level order traversal as:
	[
	  [3],
	  [9,20],
	  [15,7]
	]


	class Solution {
	public:
	vector<vector<int>> levelOrder(TreeNode* root) {
	queue<TreeNode*> node_queue;
	queue<TreeNode*> node_queue_temp;
	node_queue.push(root);
	vector<vector<int>> result;
	vector<int> temp_result;
	while(!node_queue.empty()) {
	TreeNode* now_node = node_queue.front();
	node_queue.pop();
	if(now_node != NULL) {
	temp_result.push_back(now_node->val);
	node_queue_temp.push(now_node->left);
	node_queue_temp.push(now_node->right);
	}
	if(node_queue.empty() && temp_result.size() > 0) {
	result.push_back(temp_result);
	temp_result.clear();
	queue<TreeNode*> empty;
	node_queue = node_queue_temp;
	swap(node_queue_temp, empty );
	
	}
	}
	return result;
	}
	};