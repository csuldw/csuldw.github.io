---
title: Leetcode[103]-Binary Tree Zigzag Level Order Traversal
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree {3,9,20,#,#,15,7},


	    3
	   / \
	  9  20
	    /  \
	   15   7

return its zigzag level order traversal as:

	[
	  [3],
	  [20,9],
	  [15,7]
	]


-----

思路：使用层序遍历，借助队列，依次遍历每层，如果层数为偶数，则将该层的数字翻转，使用vector的reverse函数即可。

代码Code （C++）：

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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int> > res;
        if(!root) return res;
        vector<int> nums;
        queue<TreeNode *> que;
        TreeNode *p = root;
        que.push(p);
       
        int level  = 0; // curent level
        while(!que.empty()){
            nums.resize(0);
            level += 1;
            int queSize = que.size();
            for(int i = 0; i < queSize; i++ ) {
                p = que.front();
                nums.push_back(p->val);
                que.pop();
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
            }
            if(level % 2 == 0) reverse(nums.begin(),nums.end());
            res.push_back(nums);
        }
        return  res;         
    }
};
```


</article>
