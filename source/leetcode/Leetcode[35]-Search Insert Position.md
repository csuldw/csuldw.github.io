---
title: Leetcode[35]-Search Insert Position
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/search-insert-position/

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.


	Here are few examples.
	[1,3,5,6], 5 → 2
	[1,3,5,6], 2 → 1
	[1,3,5,6], 7 → 4
	[1,3,5,6], 0 → 0


题目比较简单，直接给出答案吧
code(c++):

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        int i = 0;
        bool flag = false;
        while(i<n) {
            if( nums[i] >= target) {
                flag = true;
                break;
            }
            ++i;
        }
        return i;
    }
};
```


</article>
