---
title: Leetcode[107]-Binary Tree Level Order Traversal II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-tree-level-order-traversal-ii/

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree {3,9,20,#,#,15,7},

	    3
	   / \
	  9  20
	    /  \
	   15   7

return its bottom-up level order traversal as:

	[
	  [15,7],
	  [9,20],
	  [3]
	]


------
使用BFS层序遍历，然后得到的vector数组使用reverse反转即可。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int> > res;
        if(!root) return res;
        vector<int> nums;
        queue<TreeNode *> que;
        
        TreeNode *p = root;
        que.push(p);
        
        while(!que.empty()){ 
            int queSize = que.size();
            nums.resize(0);
            for(int i = 0; i < queSize; i++) {
                p = que.front();
                nums.push_back(p->val);
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
                que.pop();
            }
            res.push_back(nums);
        }
        reverse(res.begin(),res.end());
        return res;
        
    }
};
```


</article>
