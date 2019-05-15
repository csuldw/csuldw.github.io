---
title: Leetcode[217]-Contains Duplicate
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link：https://leetcode.com/problems/contains-duplicate/

Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.


----
思路：先将数组排序，然后从第二个开始遍历，如果和前一个值相等，则返回true，终止；此方法时间复杂度为O（nlogn），空间复杂度为O（1）

```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int n = nums.size();
        int flag = false;
        
        std::sort(nums.begin(), nums.end());
    
        int i = 1;
        while(i < n){
            if(nums[i] == nums[i-1])
                return true;
            i++;
        }
        return flag;
    }

};
```

**小记**：自己做的时候，开始使用两个for循环遍历，结果超时了，后来使用自己写的快速排序，也超时了。最后使用了std::sort(nums.begin(), nums.end())自带的sort，结果成功了！不知道是快速排序哪里出了问题。

<br><br>
拓展：快速排序算法

```
void quickSort(vector<int> &nums,int low,int high){
    int i=low,j=high;
    if(i < j){
        int po = nums[low];
        while(i < j){
            while(nums[i]<nums[j] && po < nums[j]) j--;
            if(i<j){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
            while(nums[i]<nums[j] && nums[i] < po) i++;
            if(i<j){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                j--;
            }
        }
        quickSort(nums,low,j-1);
        quickSort(nums,j+1,high);
    }
}
```

**Python代码**

```
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        n = len(nums)
        if n == 0 or n ==1:
            return False
        nums.sort()
        
        for i in range(1,n):
            if nums[i] == nums[i-1]:
                return True
        return False
```


</article>
