---
title: Leetcode[94]-Binary Tree Inorder Traversal
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-tree-inorder-traversal/

Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree `{1,#,2,3}`,

```
   1
    \
     2
    /
   3
```

return` [1,3,2]`.

-------

递归遍历法C++：

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
    vector<int> result;
    vector<int> inorderTraversal(TreeNode *root) {
        result.clear();
        inorder(root);
        return result;
    }
    void inorder(TreeNode* root){
        if(root == NULL) return;
        inorder(root->left);
        result.push_back(root->val);
        inorder(root->right);
    }
};
```


非递归算法1：

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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;  
        stack<TreeNode *> myStack;  
        if (root == NULL)  
            return result;  
        TreeNode *p = root;  
        while (p!=NULL || !myStack.empty()) {  
            while (p != NULL) {  
                myStack.push(p);  
                p = p->left;  
            }  
            if (!myStack.empty())  {  
                p = myStack.top();  
                myStack.pop();  
                result.push_back(p->val);  
                p = p->right;  
            }
        }
        return result;  
    }
};
```

非递归遍历方法二：(递归条件只需要判断栈是否为空)
在递归条件中，

- 首先取出栈顶节点，如果不为空的话，就把它的左节点进栈，然后再取栈顶元素，如果不为空，则再让它的左节点进栈，直到栈顶元素是空的节点为止；
- 然后让栈顶的空指针退栈
- 接着判断栈是否为空，如果不为空，则栈顶节点出栈，并将栈顶元素的值加入到数组中，然后把该节点的右节点入栈（右节点是否是空的，此处不做判断，内部的while循环会判断，因为内while循环后的栈顶节点必定是空指针）

最后，返回数组即可。

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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        if(root == NULL) return result; 
        
        stack<TreeNode *> stk;
        TreeNode* p = root;
        stk.push(root);
        while(!stk.empty()){
            while((p = stk.top()) && p){
                stk.push(p->left);
            }
            stk.pop();
            if(!stk.empty()){
                p = stk.top();
                result.push_back(p->val);
                stk.pop();
                p = p->right;
                stk.push(p);
            }
        }
        return result;
    }
};   
```


</article>
