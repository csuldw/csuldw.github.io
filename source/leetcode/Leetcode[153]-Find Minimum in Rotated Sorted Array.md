---
title: Leetcode[153]-Find Minimum in Rotated Sorted Array
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.


-----

C++:

方法一：直接遍历，暴力求解

```
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int min = nums[0];
        for(int  i = 1; i < n; i++) {
            if(nums[i]<min){
                min = nums[i];
            }
        }
        return min;
    }
};
```


</article>
