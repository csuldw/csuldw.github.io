---
title: Leetcode[114]-Flatten Binary Tree to Linked List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/flatten-binary-tree-to-linked-list/

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

        1
        / \
       2   5
      / \   \
     3   4   6

The flattened tree should look like:

	   1
	    \
	     2
	      \
	       3
	        \
	         4
	          \
	           5
	            \
	             6



----------

**思路: **  首先找到根节点左节点的最右子节点，然后把根节点的右子树移到左子树的最右端；接着把根结点的左子树移到右节点上，并将左子树置为空树，同时将根结点往右下移一个节点，依次递归即可。代码如下：

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
    
    void flatten(TreeNode* root) {
        if(!root) return;
        while(root){
            if(root->left && root->right){
                TreeNode *p = root->left;
                while(p->right) 
                    p =p->right;
                p->right = root->right;
            }
            
            
            if(root->left)
                root->right = root->left;
            root->left = NULL;
            root = root->right;
        }
    }
};
```


</article>
