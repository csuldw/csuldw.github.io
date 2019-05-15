---
title: Leetcode[226]-Invert Binary Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/invert-binary-tree/

Invert a binary tree.

	     4
	   /   \
	  2     7
	 / \   / \
	1   3 6   9

to

	     4
	   /   \
	  7     2
	 / \   / \
	9   6 3   1

Trivia:
This problem was inspired by this original tweet by Max Howell:
Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.


----

C++递归法求解：

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
    TreeNode* invertTree(TreeNode* root) {
        if(!root) return NULL;
        
        invertNode(root);
        return root;
    }
    
    void invertNode(TreeNode *root){
        if(root == NULL) return;
        if(!root->left && !root->right) return;
    
        TreeNode *tempNode=NULL;
        
        if(root->right) tempNode = root->right;
        if(root->left){
            root->right = root->left;
            root->left = tempNode;
        }else{
            root->left = tempNode;
            root->right = NULL;
        }
        invertNode(root->left);
        invertNode(root->right);
    
    }
};
```


</article>
