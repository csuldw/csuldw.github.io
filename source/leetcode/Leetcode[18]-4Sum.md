---
title: Leetcode[18]-4Sum
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/4sum/

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:
Elements in a quadruplet (a,b,c,d) must be in non-descending order. (ie, a ≤ b ≤ c ≤ d)
The solution set must not contain duplicate quadruplets.

------
分析：和第15道题有点类似，仅仅是多了一层循环，注意该循环的条件就行.

C++:


```
class Solution {
public:
    vector<vector<int> > fourSum(vector<int>& nums, int target) {
        vector<vector<int> > result;    
        int n = nums.size();        
        sort(nums.begin(),nums.end());
        
        for(int i = 0; i < n; i++) {
            if(i>0 && nums[i] == nums[i-1]) continue;
            for(int j = i+1; j < n; j++){
                if(j > i+1 && nums[j]== nums[j-1]) continue;
                fourNumber(nums,result,i,j,target);
            }
        }
        return result;
    }
    
    void fourNumber(vector<int> & nums,vector<vector<int> > &results, int curIndex1,int curIndex2,int target){
        int i = curIndex2 + 1;
        int j = nums.size()-1;
        
        while(i<j) {
            if(nums[curIndex1] + nums[curIndex2] + nums[i] + nums[j] < target ) i++;
            else if(nums[curIndex1] + nums[curIndex2] + nums[i] + nums[j] > target ) j--;
            else {
                vector<int> vec;
                vec.push_back(nums[curIndex1]);
                vec.push_back(nums[curIndex2]);
                vec.push_back(nums[i]);
                vec.push_back(nums[j]);
                results.push_back(vec);
                i++,j--;
                while(i < j && nums[i] == nums[i-1])i++;
                while(j > i && nums[j] == nums[j+1])j--;
            }
        }
    }
};
```


</article>
