---
title: leetcode 101. Symmetric Tree
tags: leetcode
date: 8 Apr 2017 6:24 PM
---

```
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
    bool isSymmetric(TreeNode* root) {
        if(root == NULL) {
            return true;
        }
        else {
            return isSame(root->left, root->right);
        }
        
    }
private:
    bool isSame(TreeNode* left_tree, TreeNode* right_tree) {
        if(left_tree == NULL && right_tree == NULL) {
            return true;
        }
        else if(left_tree == NULL || right_tree == NULL || left_tree->val != right_tree->val) {
            return false;
        }
        else {
            return isSame(left_tree->left, right_tree->right) && isSame(left_tree->right, right_tree->left);
        }
    }
};
```