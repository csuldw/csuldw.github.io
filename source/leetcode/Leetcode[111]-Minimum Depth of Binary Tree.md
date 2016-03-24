---
title: Leetcode[111]-Minimum Depth of Binary Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/minimum-depth-of-binary-tree/

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.



-----

C++:

递归法

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
    int minDepth(TreeNode* root) {
        
        if(root == NULL) return 0;
        if(!root->left && !root->right) return 1;
        
        int dleft=0,dright=0,dmin = 0;
        
        if(root->left) dleft = minDepth(root->left) + 1 ;
        if(root->right) dright = minDepth(root->right) + 1;
        
        if(!root->left && root->right) return dright;
        if(!root->right && root->left) return dleft;
        
        dmin = dleft>dright?dright:dleft; 
        return dmin;
        
    }
};
```




</article>
