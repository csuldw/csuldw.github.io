---
title: Leetcode[53]-Maximum Subarray
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/maximum-subarray/

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.

More practice:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

--------

给定一个长度为n的序列，求其最大子序列和。
算法思路：使用动态规划求解，dp[i]表示在尾数在位置i上的最大子序列和，那么dp[i]可以表示为

- dp[i] = max(dp[i-1] + nums[i],nums[i])
- dp[0] = nums[0]

其中dp[0]表示初值。

**C++代码如下：**
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        
        int n = nums.size();
        vector<int> dp(n);
        dp[0] = nums[0];
        int answer = dp[0];
        
        for(int i=1; i<n; ++i){
            dp[i] = max(dp[i-1]+nums[i],nums[i]);
            answer = max(answer,dp[i]);
        }
        
        return answer;
        
    }
};
```

**Python代码如下：**

```
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        ans = nums[0]
        dp = []
        dp.append(ans)
        
        for i in range(1, n):
            dp.append(max(dp[i-1] + nums[i],nums[i]))
            ans = max(dp[i], ans)
        return ans
```


</article>
