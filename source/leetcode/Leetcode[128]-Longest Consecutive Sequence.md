---
title: Leetcode[128]-Longest Consecutive Sequence
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/longest-consecutive-sequence/

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

Your algorithm should run in O(n) complexity.

---

思路：跟最大连续子序列和的思路类似，采用DP思想。设置一个长度为n的数组dp[n]，dp[i]表示在下标为i的时候，当前最大连续序列的长度为dp[i]，最后找到dp[i]中最大的那个值即可。

C++:

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int n=nums.size();
        sort(nums.begin(),nums.end());
        vector<int> dp(n);
        dp[0]=1;
        for(int i=1; i<n; i++){
            if(nums[i]==(nums[i-1]+1))dp[i]=dp[i-1]+1;
            else if (nums[i] == nums[i-1]){
                dp[i] = dp[i-1];
            }else{
                dp[i]=1;
            }
        }
        int maxl=0;
        for(int j=0; j<n;j++){
            maxl = max(maxl,dp[j]);
        }
        return maxl;
    }
};
```


</article>
