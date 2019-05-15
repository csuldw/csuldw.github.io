---
title: Leetcode[101]-Symmetric Tree
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/symmetric-tree/

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following is not:
    1
   / \
  2   2
   \   \
   3    3
```

Note:
Bonus points if you could solve it both recursively and iteratively.

-----

思路：从左右两边同时执行DFS，同步进栈出栈，如果发现不匹配，则返回false,最后如果匹配后，栈中还剩有节点，则也返回false，否则返回true。

Code（c++）：

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
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        
        TreeNode* p = root,*q = root;
        stack<TreeNode *> stk,stk2;//define two stack to save node
        stk.push(p);
        stk2.push(q);
        //if both of stk and stk2 is not empty 
        while(!stk.empty() && !stk2.empty() ){
	        //p go left,q go right
            while(p->left && q->right){
                p = p->left;
                stk.push(p);
                q = q->right;
                stk2.push(q);
            }
            //next line is very important to this method 
            if(p->left || q->right) return false;
            
            //both of stk and stk2 is not empty,pop from  stack and judge p's right node and q's left node
            if(!stk.empty() && !stk2.empty()){
                p = stk.top();
                q = stk2.top();
                stk.pop();
                stk2.pop();
                if(p->val != q->val) return false;
                
                if(p->right) stk.push(p->right);
                if(q->left) stk2.push(q->left);
            }
        }
        if(!stk.empty() || !stk2.empty()) return false;
        else return true;
    }
};
```


</article>
