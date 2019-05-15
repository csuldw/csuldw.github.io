---
title: Leetcode[15]-3Sum
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/3sum/

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)
The solution set must not contain duplicate triplets.
    For example, given array S = {-1 0 1 2 -1 -4},

    A solution set is:
    (-1, 0, 1)
    (-1, -1, 2)

----------

分析：根据题意，我们可以要找出三个数相加等于0的这样的一个集合，所以采用二维数组存储。

首先对数组进行从小到大排序，然后抽取一个变量出来，该变量从左往右递归遍历，递归的同时设置两个变量，让其中一个从第一个变量的右边开始，另一个从数组的末端开始，同步的向中间遍历，有点类似于快速排序的判断方式，

- 如果三个数相加小于零，则让第二个变量自加；
- 如果三个数相加大于零，则让第三个变量自减；
- 如果三个数相加等于零，则将三个数加入到数组中，然后让第二个变量和第三个变量同步增减，自增自减的过程中要判断是否有重复数字；

依次递归，直到第一个变量条件终止为止。


**Code(c++):**

```
class Solution {
public:
    vector<vector<int> > threeSum(vector<int> &nums) {
        vector<vector<int> >  result;

        sort(nums.begin(), nums.end());
    
        for(int i = 0; i < nums.size(); i++){
            if(i > 0 && nums[i] == nums[i-1]) continue;
            threeNumber(nums, result, i); 
        }
        return result;
    }
    //return vector<vector<int> > results 
    void threeNumber(vector<int> &nums, vector<vector<int> > &results, int curIndex) {
        int i = curIndex + 1;
        int j = nums.size()-1;
    
        while(i < j){
            if(nums[curIndex] + nums[i] + nums[j] < 0)   i++;
            else if(nums[curIndex] + nums[i] + nums[j] > 0)   j--;
            else{
                vector<int> v;
                v.push_back(nums[curIndex]);
                v.push_back(nums[i]);
                v.push_back(nums[j]);
                results.push_back(v);
                i++; j--;
                while(i < j && nums[i]==nums[i - 1]) i++;
                while(j > i && nums[j] == nums[j + 1]) j--;
            }
        }
    }
};
```


</article>
