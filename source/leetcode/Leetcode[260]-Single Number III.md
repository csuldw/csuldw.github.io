---
title: Leetcode[260]-Single Number III
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">



Link: https://leetcode.com/problems/single-number-iii/

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

For example:

Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

Note:
The order of the result is not important. So in the above example, [5, 3] is also correct.
Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

---

思路：首先对数组进行排序，然后一对一对的进行比较，如果两个相等，则以步长为2往后移动，如果不相等，则将当前的值加入到返回变量中，然后以步长为1往后移动。

C++:

```
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<int> res;
        int n = nums.size();
        int i = 0;
        while(i<n-1){
            if(nums[i]==nums[i+1]){
                i+=2;
            }else{
                res.push_back(nums[i]);
                i+=1;
            }
        }
        if(nums[n-1]!=nums[n-2]){
            res.push_back(nums[n-1]);
        }
            
        return res;
    }
};
```


</article>
