---
title: Leetcode[222]-Count Complete Tree Nodes
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/count-complete-tree-nodes/

Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.



------

思路：分别计算左右子树，然后返回左右字数节点个数加一

递归法：（超时了）

```
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == NULL) return 0;
        
        int lcount=0,rcount=0;
        if(root->left != NULL){
            lcount = countNodes(root->left);
        }
        if(root->right != NULL){
            rcount = countNodes(root->right);
        }
        return lcount+rcount+1;  
    }
};
```

方法二：
先计算左右的深度是否相等，相等则为满二叉树，满二叉树的节点个数为深度的平方减一,即depth^2-1；如果不相等，则递归以同样的方式计算左子树和右子树，并返回两者个数之和加一。

**Code(c++):**

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
    int countNodes(TreeNode* root) {
        if(root == NULL) return 0;
		
        int ldepth = getLeftDepth(root);
        int rdepth = getRightDepth(root);
        //return the square of leftdepth -1
        if(ldepth == rdepth) return (1 << ldepth) - 1;
    
        return countNodes(root->left)+countNodes(root->right)+1;
    }
    //compute the depth of left tree
    int getLeftDepth(TreeNode *root){
        int depth = 0;
        while(root){
            depth++;
            root = root->left;
        }
        return depth;
    }
    //compute the depth of right tree
    int getRightDepth(TreeNode *root){
        int depth = 0;
        while(root){
            depth++;
            root = root->right;
        }
        return depth;
    }
};
```


</article>
