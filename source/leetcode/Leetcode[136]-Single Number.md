---
title: Leetcode[136]-Single Number
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Lind:https://leetcode.com/problems/single-number/

Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

-----

分析：首先对数组进行排序，然后从开始进行两两比较，如果两个相等，则以步长为2往后比较，如果不相等，则返回第一个值。代码如下：

Code(c++)

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        
        sort(nums.begin(),nums.end());
        if(n==1) return nums[0];
        int i = 0;
        while(i<n) {
            if(nums[i] == nums[i+1])
                i = i+2;
            else
                return nums[i];
        }
        return 0;
    }
};
```


</article>
