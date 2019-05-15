---
title: Leetcode[110]-Balanced Binary Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/balanced-binary-tree/

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

-------

判断一棵树是否属于平衡二叉树

判断主节点的左右节点深度大小差，如果不在【-1,1】内，返回false，否则，继续判断其左右节点是否属于平衡二叉树；

只要有不满足的，就返回false

Code(C++):

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
    bool isBalanced(TreeNode* root) {
        if(root == NULL) return true;
        
        int ldepth = getDepth(root->left);
        int rdepth = getDepth(root->right);
        
        if(ldepth - rdepth > 1 || ldepth - rdepth < -1) return false;
        
        return isBalanced(root->left) && isBalanced(root->right);
    }
    
    //get node's depth
    int getDepth(TreeNode *root){
        if(root==NULL) return 0;
    
        int ldepth=0,rdepth=0;
        ldepth = getDepth(root->left);
        rdepth = getDepth(root->right);
 
        return ldepth > rdepth ? ldepth+1:rdepth+1;
        
    }
};
```




</article>
