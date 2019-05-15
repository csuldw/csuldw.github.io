---
title: Leetcode[283]-Move Zeroes
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/move-zeroes/

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:

- You must do this in-place without making a copy of the array.
- Minimize the total number of operations.

---

C++代码：

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n=nums.size();
        if (n==0 || n==1) return;
        int i=0,s = n-1;
        while(i<=s){
            if(nums[i]==0){
                for(int j = i; j<s; j++){
                    nums[j] = nums[j+1];
                }
                nums[s]=0;
                s = s-1;
            }else{
                i=i+1;
            }
        }
    }
};
```


</article>
