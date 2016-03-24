---
title: Leetcode[104]-Maximum Depth of Binary Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/maximum-depth-of-binary-tree/

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.


--------

求最大深度，用递归的方法

C++:

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
    int maxDepth(TreeNode* root) {
        
        if(root==NULL) return 0;
        int ldepth = 0,rdepth = 0;
        if(root->left!=NULL){
            ldepth = maxDepth(root->left);
        }
        if(root->right!=NULL){
            rdepth = maxDepth(root->right);
        }
        return ldepth>rdepth?ldepth+1:rdepth+1;   
    }
};
```


</article>
