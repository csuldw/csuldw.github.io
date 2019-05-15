---
title: Leetcode[113]-Path Sum II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/path-sum-ii/

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,


              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1

return


	[
	   [5,4,11,2],
	   [5,8,4,5]
	]


------------

思路：使用深度优先遍历，需要

- 一个map，保存节点是否出栈过；
- 一个一维数组，保存根结点到当前节点的路径；
- 一个二维数组，保存根结点到当前叶节点的和等于给定sum的路径集合；
- 一个栈，用来辅助深度优先遍历；

按照深度优先的遍历方式遍历，

- 进栈的时候元素同时添加到一维数组中，并将该节点的map值设置为0,
- 当节点左右节点都为空的时候，判断一维数组里面的数据和是否等于给定sum，如果等于就把它丢到二维数组中，同时执行下一步出栈；
- 出栈的时候也同时缩小一维数组的长度，并将该节点的map值设置为1，表示该节点不能再次进栈了；

Code（c++）：非递归算法：

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
    vector<vector<int> > pathSum(TreeNode* root, int sum) {
        vector<vector<int> > res;
        if(root==NULL) return res;
        vector<int> nums;
        map<TreeNode *, int> visited;
        stack<TreeNode *> stk;
        
        TreeNode* p = root;
        nums.push_back(p->val);
        stk.push(p);
        visited[p] = 0;
        
        while(!stk.empty()){
            p = stk.top();
            while(p->left && visited[p] == 0){
	            //this is very important to break this while
                if(visited[p->left]==1) break;
                p = p->left;
                nums.push_back(p->val);
                stk.push(p);
                visited[p] = 0;
            }
            if(!stk.empty()){
                p = stk.top();
                if(!p->left && !p->right && sumVector(nums) == sum){
                    res.push_back(nums);
                }
                if( p->right && (visited.find(p->right) == visited.end() || visited[p->right] == 0)){
                    p = p->right;
                    stk.push(p);
                    nums.push_back(p->val);
                    visited[p] = 0;
                    continue;
                }
                visited[p] = 1;
                stk.pop();
                nums.resize(nums.size()-1);
            }
        }
        return res;
    }
    
    int sumVector(vector<int> & nums){
        int n = nums.size();
        if( n == 0 ) return 0;
        int sum = 0;
        for(int i = 0; i < n; i++) {
            sum += nums[i];
        }
        return sum;
    }
};

```


递归方法：

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
    vector<vector<int> > pathSum(TreeNode* root, int sum) {
        vector<vector<int> > result;
        vector<int> nums;
        getPath(result,nums,sum,0,root);
        return result;

    }
    
    void getPath(vector<vector<int> > &res, vector<int> &nums, int target, int sum, TreeNode *root) {
        
        if(root==NULL) return;
        sum += root->val;
        nums.push_back(root->val);
        if(sum == target && !root->left && !root->right){
            res.push_back(nums);
        }
        
        getPath(res, nums, target, sum, root->left);
        getPath(res, nums, target, sum, root->right);
        
        nums.pop_back();
        sum -= root->val;
    }
};
```


</article>
