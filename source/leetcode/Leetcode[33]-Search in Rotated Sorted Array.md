---
title: Leetcode[33]-Search in Rotated Sorted Array
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/search-in-rotated-sorted-array/

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

--------

C++:

方法一：直接for循环求解

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        
        for(int i = 0; i<n; i++){
            if(nums[i] == target) return i;
        }
        return -1;
    }
};
```

方法二：二分搜索求解
分析：从中点处划分，左右两边一定有一部分是有序的，对于有序的分别做判断。

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        
        int i = 0, j = nums.size()-1;
        
        while(i<=j){
            int mid = (i+j)/2;
            
            if(nums[mid] == target) 
                return mid;
            //if left is sorted
            else if(nums[i] <= nums[mid]){
                if(nums[i] <= target && target < nums[mid]) 
                    j = mid-1;
                else
                    i = mid + 1;
            }else {
                if(nums[mid] < target && target <= nums[j] ) 
                    i = mid +1;
                else 
                    j = mid-1;
            }
        }
        return -1;
    }
};
```


</article>
