---
title: Leetcode[70]-Climbing Stairs
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/climbing-stairs/

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

------

题意：给你一个n阶的台阶，你一次最多只能上2个台阶，请问一共有多少个走法？

分析：典型的斐波那契数列题目，可以使用递归求解，也可以使用DP方法求解；

i表示阶梯数，f(i)表示有多少种走法；

- 当i = 1时，f(1) = 1,
- 当i = 2时，f(2) = 2;
- 当i > 3时，f(i) = f(i-1) + f(i-2)。

其实很容易理解，当阶梯数大于2时，它的走法可以是从n-1阶梯走一步，或是从n-2阶梯处一次走两步到达，即f(i) = f(i-1) + f(i-2)。

```
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n);
        for(int i = 0; i < n; i++){
            if(i < 2) dp[i] = i + 1;
            else dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n-1];
    }
};
```


</article>
