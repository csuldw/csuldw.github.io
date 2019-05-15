---
title: Leetcode[1]-Two Sum
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/two-sum/  

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

------
分析：
方法一：使用两个for循环，依次比较，不过这个方法在leetcode上超时了

c++

```
vector<int> twoSum(vector<int>& nums, int target) {
    vector<int> index(2);
    int n = nums.size();
    for(int i = 0; i < n ; i++) {
        index[0] = i+1;
        for(int j = i+1 ; j < n ; j++) {
            if(nums[i] + nums[j] == target){
                index[1] = j+1;
                return index;
            }
        }
    }
    return index;
}
```

法二：使用map存储所有的数组值和下标值，然后循环在map中找看能否找到target-nums[i]的map，如果找到了就终止循环，没找到继续找；最后返回index数组；

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> index(2);
        int n = nums.size();
        map<int,int> mapv;
        for(int i = 0; i < n ; i++) {
            mapv[nums[i]] = i;
        }
        map<int,int>::iterator it;
        for(int i = 0; i < n; i++) {
            it = mapv.find(target - nums[i]);
            if(it != mapv.end() && i!=it->second){
                index[0]=min(i+1, it->second + 1);
                index[1]=max(i+1, it->second + 1);
                break;
            }
        }
        return index;
    }
};
```


</article>
