---
title: Leetcode[162]-Find Peak Element
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/find-peak-element/

A peak element is an element that is greater than its neighbors.

Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that num[-1] = num[n] = -∞.

For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.

click to show spoilers.


-----

分析：

- 如果只有一个元素，直接返回0；
- 如果元素个数>=2，判断首尾是否大于它的附近元素，大于则返回下标；
- 循环遍历，下标从>=1到 < n-1，判断nums[i]是否同时大于它的附近元素，如果大于则返回该下标；
 
否则，返回-1；


Code(c++)

```
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        if(n==1) return 0;
        if(nums[0] > nums[1]) return 0;
        if(nums[n-1] > nums[n-2]) return n-1;
        
        for(int i=1; i<n-1; i++){
            if(nums[i]>nums[i-1] && nums[i]>nums[i+1]){
                return i;
            }
        }
        return -1;
    }
};
```


</article>
