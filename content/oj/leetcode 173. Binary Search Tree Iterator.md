---
title: leetcode 173. Binary Search Tree Iterator
tags: leetcode, oj
date: 25 Apr 2017 10:20 PM
---
	Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.
	
	Calling next() will return the next smallest number in the BST.
	
	Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.


	/**
	 * Definition for binary tree
	 * struct TreeNode {
	 *     int val;
	 *     TreeNode *left;
	 *     TreeNode *right;
	 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
	 * };
	 */
	class BSTIterator {
	private:
	    stack<TreeNode*> st;
	
	    /** put all the left child() of root */
	    void find_left(TreeNode* root)
	    {
	        TreeNode* p = root;
	        while (p != NULL)
	        {
	            st.push(p);
	            p = p->left;
	        }
	    }
	public:
	    BSTIterator(TreeNode *root) {
	        find_left(root);
	    }
	
	    /** @return whether we have a next smallest number */
	    bool hasNext() {
	        return !st.empty();
	    }
	
	    /** @return the next smallest number */
	    int next() {
	        TreeNode* top = st.top();
	        st.pop();
	        if (top->right != NULL)
	            find_left(top->right);
	
	        return top->val;
	    }
	
	};
	
	/**
	 * Your BSTIterator will be called like this:
	 * BSTIterator i = BSTIterator(root);
	 * while (i.hasNext()) cout << i.next();
	 */