---
title: Leetcode[4]-Median of Two Sorted Arrays
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/median-of-two-sorted-arrays/

There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).


----

思路：先将两个数组合并，然后排序，最后找中位数

中位数：若有n个数，n为奇数，则选择第（n+1）/2个为中位数，若n为偶数，则中位数是（n/2以及n/2+1）的平均数；

合并两个vector：merge(nums1.begin(),nums1.end(),nums2.begin(),nums2.end(),nums.begin());


Code(c++):

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(),n = nums2.size();
        vector<int> nums(m+n);
    
        merge(nums1.begin(),nums1.end(),nums2.begin(),nums2.end(),nums.begin());
    
        sort(nums.begin(),nums.end());
    
        int mid = (m+n)/2;
        if((m+n)%2==0) {
            return double(nums[mid]+nums[mid - 1])/2;
        }else
            return nums[mid];
    }
};
```




</article>
