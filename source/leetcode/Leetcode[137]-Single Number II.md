---
title: Leetcode[137]-Single Number II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/single-number-ii/

Given an array of integers, every element appears three times except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

---

思路：先排序，然后一对一对的进行比较.

C++

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        if(n==1) return nums[0];
        sort(nums.begin(),nums.end());
        int i = 0;
        while(i<n-2){
            if(nums[i]!=nums[i+1]){
                return nums[i];
            }else{
                i = i + 3;
            }
        }
        return nums[n-1];
    }
};
```


</article>
