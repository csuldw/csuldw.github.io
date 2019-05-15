---
title: Leetcode[145]-Binary Tree Postorder Traversal
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-tree-postorder-traversal/

Given a binary tree, return the postorder traversal of its nodes' values.

For example:
Given binary tree `{1,#,2,3}`,

	   1
	    \
	     2
	    /
	   3

return `[3,2,1]`.

Note: Recursive solution is trivial, could you do it iteratively?

-------


方法一：递归遍历

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

    vector<int> nums;
    vector<int> postorderTraversal(TreeNode* root) {
        postorder(root);
        return nums;
    }
    
    void postorder(TreeNode * root){
        if(root==NULL) return;
        if(root->left) postorder(root->left);
        if(root->right) postorder(root->right);
        nums.push_back(root->val);
    }
};
```


</article>
