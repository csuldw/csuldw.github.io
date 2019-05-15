---
title: Leetcode[27]-Remove Element
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/remove-element/

Given an array and a value, remove all instances of that value in place and return the new length.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

-------
思路：遍历数组，如果数组对应的数等于给定的值，数组最后一位和当前位互换，然后将数组长度减一；否则，就执行i++;最后重置数组长度。

Code(c++):
```
class Solution {
public:

    void swap(int *a, int *b){
        int temp = *a;
        *a = *b;
        *b = temp;
    }
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        if(n == 0) return 0;
        int i = 0;
        while( i < n){
            if(nums[i] == val) {
                swap(&nums[i],&nums[n-1]);
                n--;
            }else{
                i++;
            }
        }
        nums.resize(n);
        return n;
    }
};
```


</article>
