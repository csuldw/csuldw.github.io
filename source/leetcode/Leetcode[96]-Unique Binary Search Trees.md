---
title: Leetcode[96]-Unique Binary Search Trees
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/unique-binary-search-trees/

Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.

	   1         3     3      2      1
	    \       /     /      / \      \
	     3     2     1      1   3      2
	    /     /       \                 \
	   2     1         2                 3



**思路：** 动态规划解题，dp[n]表示n个节点可以有dp[n]种不同的树，不管n为多少，先固定一个节点，剩余n-1个节点，分配给左右字数，然后把左子树个数乘以右子树的个数，

- 初始值dp[0] = 1,dp[1] = 1,
- dp[n] = dp[0] * dp[n-1] + dp[1] * dp[n-2] + ...+ dp[i] * dp[n-1-i] +... + dp[n-1] * dp[0]，也就是左边i个节点，右边n-1-i个节点。代码如下：

**Code(c++):**

```
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp;
        dp.resize(n+1);//set  the length of vector to n+1
        for(int i = 0; i <= n; i++) {
	        //dp[0] = 1 , dp[1] =1
            if(i<2){
                dp[i] = 1;
                continue;
            }
            //dp[n] = dp[0]*dp[n-1]+dp[1]*dp[n-2]+...+dp[n-1]*dp[0]
            for(int j = 1; j<=i; j++) {
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
};
```


</article>
