---
title: leetcode 501. Find Mode in Binary Search Tree
tags: leetcode, oj
date: 25 Apr 2017 11:14 PM
---
	Given a binary search tree (BST) with duplicates, find all the mode(s) (the most frequently occurred element) in the given BST.
	
	Assume a BST is defined as follows:
	
	The left subtree of a node contains only nodes with keys less than or equal to the node's key.
	The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
	Both the left and right subtrees must also be binary search trees.
	For example:
	Given BST [1,null,2,2],
	   1
	    \
	     2
	    /
	   2
	return [2].
	
	Note: If a tree has more than one mode, you can return them in any order.

解法1:不考虑二叉查找树的特性，遍历查找添加到map，空间复杂度为O(n)
 
	class Solution {
	public:
	    unordered_map<int,int> umap;
	    int maximum = 0;
	    void preorder(TreeNode* root) {
	        if(root == NULL) {
	            return ;
	        }
	        // 如果在原map中没有索引，则加入map
	        if(umap.find(root->val) == umap.end())
	            umap[root->val] = 0;
	        else {
	            umap[root->val]++;
	            maximum = max(maximum, umap[root->val]);
	        }
	
	        preorder(root->left);
	        preorder(root->right);
	    }
	    vector<int> findMode(TreeNode* root) {
	        umap.clear();
	        maximum = 0;
	        vector<int> ans;
	        if(root == NULL)
	            return ans;
	
	        preorder(root);
	
	        for(auto itr = umap.begin();itr != umap.end();itr++)
	            if(itr->second == maximum) // 如果值为最大值，则获取该索引
	                ans.push_back(itr->first);
	
	        return ans;
	
	    }
	};
 
解法2: 考虑其特性，从左到右递增，二叉搜索树的中序遍历的结果恰好是所有数的递增序列，所以采用中序遍历，只需存储当前值就可以，不需要额外的空间
	class Solution {
	public:
	    vector<int> result;
	    int maxCount = 0, currentVal, tempCount = 0;
	    void inorder(TreeNode* root) {
	        if (root == NULL) 
	            return;
	        // 从左子树开始
	        inorder(root->left);
	        tempCount++;
	        if (root->val != currentVal) {
	            currentVal = root->val;
	            tempCount = 1;
	        }
	        if (tempCount > maxCount) {
	            maxCount = tempCount;
	            // 有大于原来的清空，重新来
	            result.clear();
	            result.push_back(root->val);
	        } 
	        else if (tempCount == maxCount) {
	            result.push_back(root->val);
	        }
	        inorder(root->right);
	    }
	    vector<int> findMode(TreeNode* root) {
	        inorder(root);
	        return result;
	    }
	
	};