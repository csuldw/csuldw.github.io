---
title: Leetcode[98]-Validate Binary Search Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/validate-binary-search-tree/

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
confused what "{1,#,2,3}" means? > read more on how binary tree is serialized on OJ.

-----

思路：递归方法，对于根结点

- 如果有左子树，比较根结点与左子树的最大值，如果小于等于则返回false；
- 如果有右子树，比较根结点与右子树的最小值 ，如果大于等于则返回false；
- 接着判断左子树和右子树是否也是合法的二叉搜索树；

Code(c++):

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
    bool isValidBST(TreeNode* root) {
        if(root == NULL) return true;
        if(root->left && root->val <= leftMax(root->left)) return false;
        if(root->right && root->val >= rightMin(root->right)) return false;
        
        return isValidBST(root->left) && isValidBST(root->right);
    }
    //get the left tree max value
    int leftMax(TreeNode *root){
        if(root == NULL) return INT_MIN;
        int max = root->val;
    
        int lmax = leftMax(root->left);
        int rmax = leftMax(root->right);
    
        int max1 = lmax>rmax?lmax:rmax;
        return max>max1?max:max1;
    }
    //get the right tree minimum value
    int rightMin(TreeNode *root){
        if(root == NULL) return INT_MAX;
        int max = root->val;
    
        int lmin = rightMin(root->left);
        int rmin = rightMin(root->right);
    
        int max1 = lmin>rmin? rmin:lmin;
        return max>max1?max1:max;
    }
    
};
```


</article>
