---
title: Leetcode[100]-Same Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/same-tree/

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

-----

思路：递归法判断
假设两棵树根结点为p和q

- 如果p和q都为空，则返回true；否则，
- 如果p和q都非空，并且他们的值都相等，则判断其左右子树是否是相似树；否则
- 返回false；

代码如下(C++)：

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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q) return true;
        else if(p && q && (p->val == q->val)){
            return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
        }else{
            return false;
        }
    }
};
```


</article>
