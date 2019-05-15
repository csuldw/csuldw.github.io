---
title: Leetcode[300]-Longest Increasing Subsequence
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link：https://leetcode.com/problems/longest-increasing-subsequence/


Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given `[10, 9, 2, 5, 3, 7, 101, 18]`,
The longest increasing subsequence is `[2, 3, 7, 101]`, therefore the length is `4`. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

Follow up: Could you improve it to O(n log n) time complexity?

---

这道题和最长连续子序列有些区别，它不限制【连续】这个条件，只要递增即可。

思路：定义一个长度为n的int类型一维数组dp[n]，dp[i]用来表示第i个位置上的最长递增子序列长度。dp[i]的计算过程如下：

- 初始化dp[0]=1；
- 当i>0时，设置一个变量max_dp，初始值为1,记录的是以当前位置结尾时的最长递增子序列长度。通过循环遍历前面的i-1个数，如果位置i的数大于前面位置j的数，就比较max_dp和dp[j]+1,将大的值赋值给max_dp，最后将max_dp赋值给dp[i]；
- 最后，遍历dp[n]，找出最大的值，即为最长递增子序列的长度。


C++代码：


```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        vector<int> dp(n);
        dp[0]=1;
        for(int i=1; i<n; i++){
            int j=i-1,max_dp=1;            
            while(j>=0){
                if(nums[j]<nums[i]){
                    max_dp=(max_dp>(dp[j]+1)?max_dp:(dp[j]+1));
                }
                --j;
            }
            dp[i]=max_dp;
        }
        int max=0;
        for(int k=0;k<n;k++){
            max=(max>dp[k]?max:dp[k]);
        }
        return max;
    }
};
```


</article>
