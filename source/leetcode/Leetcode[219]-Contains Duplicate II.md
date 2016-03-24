---
title: Leetcode[219]-Contains Duplicate II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/contains-duplicate-ii/

Given an array of integers and an integer k, find out whether there there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

-------
**分析**：用C++中的map记录num和下标value，key值为数值，value为值在nums数组中的下标。首先遍历数组，如果在map中存在
【mapv.find(number) != mapv.end()】并且当前位置和map中找到的位置差小于等于k【i-mapv[number] <= k】，就返回true，不然就将该值加入到map中。依次循环...

Code(c++)

```
class Solution {  
public:  
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n = nums.size();
        map<int, int> mapv;
        for (int i = 0; i < n; i++){
            int number = nums[i];
            if (mapv.find(number) != mapv.end() && i-mapv[number] <= k){
                return true;
            }else{
                mapv[number] = i;
            }
        }
        return false;
    }
};  

```


</article>
