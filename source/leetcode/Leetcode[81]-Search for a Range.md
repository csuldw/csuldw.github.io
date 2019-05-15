---
title: Leetcode[81]-Search for a Range
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/

Given a sorted array of integers, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].

-------------

思路：首先初始化一个2列的数组，值为-1，然后一次遍历数组，设置一个变量作为标识，记录出现target值的下标，并保存到数组中，如果标识值等于=了，就不增加它的值，保证数组第二个元素是最后一个出现target的下标。

C++:

```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res(2);
        res[0]=-1,res[1]=-1;
        int n = nums.size();
        
        int temp = 0;
        for(int i = 0; i < n; i++){
            if(target == nums[i]){
                if(temp == 2)
                    res[temp-1] = i;
                else
                    res[temp++] = i;
            }
        }
        if(temp ==1) {
            res[temp] = res[0];
        }
        return res;
    }
};
```


</article>
