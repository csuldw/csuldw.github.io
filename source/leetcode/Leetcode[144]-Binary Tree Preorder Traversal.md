---
title: Leetcode[144]-Binary Tree Preorder Traversal
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-tree-preorder-traversal/

Given a binary tree, return the preorder traversal of its nodes' values.

For example:
Given binary tree` {1,#,2,3}`,

	   1
	    \
	     2
	    /
	   3

return [1,2,3].



-----------


##  C++递归遍历：

```
/**C++
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
    vector<int> nums;
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == NULL) return nums;
        preOrder(root);
        return nums;
    }
    void preOrder(TreeNode * root){
        if(root == NULL) return;
        nums.push_back(root->val);
        if(root->left)  preOrder(root->left);
        if(root->right) preOrder(root->right);
    }   
};
```

## C++递归遍历二:

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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ves;
        preorder(root, ves);
        return ves;
    }
    void preorder(TreeNode* root, vector<int> &nums){
        if(!root) return;
        nums.push_back(root->val);
        preorder(root->left, nums);
        preorder(root->right, nums);
    }   
};
```

## 非递归遍历C++：

```
vector<int> preorderTraversal(TreeNode* root) {
    vector<int > nums;
    stack<TreeNode* > stk;
    if(root == NULL) return nums;
    TreeNode * p = root;
    stk.push(p);   
    while(!stk.empty()){
        p = stk.top();
        nums.push_back(p->val);
        stk.pop();
        if(p->right) stk.push(p->right);
        if(p->left) stk.push(p->left);
    }
}
```


</article>
