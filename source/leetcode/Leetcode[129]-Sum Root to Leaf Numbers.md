---
title: Leetcode[129]-Sum Root to Leaf Numbers
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/sum-root-to-leaf-numbers/

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

```
    1
   / \
  2   3
```

The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.

--------

这道题让我真的醉了，先说下思路吧。

先求出每个叶子节点的值，然后将这些值相加。求叶子节点的值可以通过深度优先遍历，一遍往下，一遍增加各个节点的值，如果是叶子节点就将数组值的和加入到一个只有叶子节点的数组中，最后求这个只有叶子节点的数组的值得和。从头到尾自己写的代码，可以AC了，还没有进行优化，先做到这里吧。

代码（C++）：

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
	//return sum of newleafValue
    int sumNumbers(TreeNode* root) {
        if(root == NULL) return 0;
        if(!root->left && !root->right) return root->val;
    
        vector<int> nums = saveLeafNums(root);
        return sumV(nums);
    }
    //save leftnode to vector
    vector<int> saveLeafNums(TreeNode * root){
    
        vector<int> nums,nodeValue;
        int eachSum = 0;
        stack<TreeNode *> stk;
        map<TreeNode *, int> visited;
    
        TreeNode *p = root;
    
        stk.push(p);
        visited[p] = 0;
        nodeValue.push_back(p->val);
    
        while(!stk.empty()){
            while(p->left && visited[p]==0 ){
	            //if this node is visited,break 
                if(visited[p->left]==1) break;
                p = p->left;
                stk.push(p);
                visited[p] = 0;
                tenMutilVector(nodeValue);//enlarge vector element's value
                nodeValue.push_back(p->val);
            }
            if(!stk.empty()) {
                p = stk.top();
                if(!p->left && !p->right){
                    nums.push_back(sumV(nodeValue));
                }
                if(p->right && (visited.find(p->right)==visited.end() || visited[p->right]==0)){
                    p = p->right;
                    stk.push(p);
                    visited[p] = 0;
                    tenMutilVector(nodeValue);
                    nodeValue.push_back(p->val);
                    continue;
                }
                visited[p] = 1;
                stk.pop();
                tenDivideVector(nodeValue);
                nodeValue.resize(nodeValue.size()-1);
            }
        }
        return nums;
    }
    
    void tenMutilVector(vector<int> &nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++){
            nums[i] *= 10;
        }
    }
    
    void tenDivideVector(vector<int> &nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++){
            nums[i] /= 10;
        }
    }
    
    int sumV(vector<int> &nums){
        int n = nums.size();
        int sumValue = 0;
        for(int i = 0; i < n; i++){
            sumValue += nums[i];
        }
        return sumValue;
    }   
};
```


附加一个求深度的代码：

```C++
    int getDepth(TreeNode* root) {
        if(root == NULL) return 0;
    
        int ldepth = 0,rdepth = 0;
    
        ldepth = getDepth(root->left);
        rdepth = getDepth(root->right);
    
        return ldepth>rdepth?ldepth+1:rdepth+1;
    }
```


方法二：（递归法）

```
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        sum = 0;
        sumStep(root, 0);
        return sum;
    }
private:
    int sum;
    void sumStep(TreeNode *root, int num){
        if(!root) return;
        if(!root->left && !root->right) {
            sum = sum + num*10 + root->val;
        }else{
            sumStep(root->left, num*10 + root->val);
            sumStep(root->right, num*10 + root->val);
        }
        
    }
};
```


</article>
