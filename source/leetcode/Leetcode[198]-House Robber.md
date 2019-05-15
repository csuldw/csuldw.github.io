---
title: Leetcode[198]-House Robber
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/house-robber/

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

-------

题目意思：假如现在你是一个强盗，一个武功超群，足智多谋的江洋大盗，现在你要去一条街上抢劫，这条街全是贪官污吏，一共有N家，每家都有一定数量的金子，如果相邻的两家在同一个晚上都被打劫了，那么就会有一个类似触发器的东西自动调动兵马来街上抓你。

请问，在不触发这个警报器的前提下，你能抢劫到多少money？


**动态规划思想解题**

**分析：**，有N家贪官，假设是从左到右，第i家贪官家里的钱数为m[i]，i从0到N-1，根据题意可知，肯定不能在今晚打劫两个相邻的贪官，也就是假如打劫了第i家，就不能打劫第i-1家和i+1家。

设dp[i]表示我从第1家到达第i家能强盗的最大money数；

- 当你打劫第一家的时候，i = 0，可以得到的钱dp[i] = m[0]；
- 当你到达第二家的时候，i = 1，此时能得到的钱数为max(m[0],m[1]),因为不能同时打劫第一家和第二家；
- 当到达第i家的时候，i＞＝２，此时能够得到的钱数应该为max{dp[i-1],dp[i-2]+m[i]},即要么是到达上一家时的最大money数，要么是到达上上家时的最大money+这一家的money数，两者中较大的那个；

所以最后我们要得到的就是dp[N-1]，即到达最后一家时的最大money数。

-------

Code(c++):

```
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 0;
        vector<int> dp(n);
        for(int i = 0; i < n; i++) {
            if(i == 0) dp[i] = nums[i];
            else if(i == 1) dp[i] = max(nums[i],nums[i-1]);
            else{
                dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
            }
        }
        return  dp[n-1];
    }
};
```


</article>
