---
title: Leetcode[173]-Binary Search Tree Iterator
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/binary-search-tree-iterator/

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

------

**思路：** 遍历一遍，然后从小到大的放到队列里去，然后判断队列是否非空，不非空则前面的就是最小的，出队即可！

C++:

```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
public:
    queue<int> que;
    BSTIterator(TreeNode *root) {
        stack<TreeNode *> stk;
        map<TreeNode *, int> visited;
        TreeNode *p;
        if(root) { 
            stk.push(root);
            visited[root] = 0;
        }
        while(!stk.empty()){
            p = stk.top();
            while(p->left && visited[p] == 0){
                if(visited[p->left] == 1) break;
                p = p->left;
                stk.push(p);
                visited[p]=0;
            }
            visited[p]=1;
            que.push(p->val);
            stk.pop();
            
            if(p->right && (visited.find(p->right)==visited.end() || visited[p->right]==0)){
                stk.push(p->right);
                visited[p->right] = 0;
                continue;
            }
        }
        
        
        
	/* another way     
	stack<TreeNode*> stk;
        while(root || !stk.empty() ) {
            if(root){
                stk.push(root);
                root = root->left;
            }else {
                root = stk.top();
                stk.pop();
                que.push(root->val);
                root = root->right;
            }   
        } */ 
    }

/** @return whether we have a next smallest number */
    bool hasNext() {
        if(!que.empty()) return true;
        return false;
    }
/** @return the next smallest number */
    int next() {
        int val = que.front();
        que.pop();
        return val;
    }
};

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = BSTIterator(root);
 * while (i.hasNext()) cout << i.next();
 */
```


</article>
