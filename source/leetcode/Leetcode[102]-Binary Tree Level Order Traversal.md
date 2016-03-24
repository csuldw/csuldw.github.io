---
title: Leetcode[102]-Binary Tree Level Order Traversal
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-tree-level-order-traversal/

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree {3,9,20,#,#,15,7},


	    3
	   / \
	  9  20
	    /  \
	   15   7

return its level order traversal as:


	[
	  [3],
	  [9,20],
	  [15,7]
	]


-----

**思路**： 使用层序遍历，一层一层的出队，并将节点值放入数组中；

代码C++：

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
    vector<vector<int> > levelOrder(TreeNode* root) {
        vector<vector<int> > res;
        vector<int> nums;
        if(root == NULL) return res;
        queue<TreeNode *> que;
        TreeNode *p = root;
        que.push(p);
        while(!que.empty()){
            int queSize = que.size();
            nums.resize(0);
            for(int i = 0; i < queSize; i++){
                p = que.front();
                nums.push_back(p->val);
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
                que.pop();
            }
            res.push_back(nums);
        }
        return res;
    }
};
```  


有问题，来一起讨论吧~


</article>
