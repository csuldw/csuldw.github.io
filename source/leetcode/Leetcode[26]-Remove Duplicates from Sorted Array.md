---
title: Leetcode[26]-Remove Duplicates from Sorted Array
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/remove-duplicates-from-sorted-array/

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

--------

**思路**：需要额外添加一个新的变量pos，初始值为0，用来记录非重复元素的位置。从数组的第二个元素开始遍历，如果和前面的元素相等，则直接跳到下一个；如果不等，则将该数组的值赋值给++pos位，接着继续遍历下一个；

**Code(c++):**

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        int pos = 0,i = 1;
        while(i < n){
            if(nums[i] == nums[i-1]){
                i++;
            }else{
                nums[++pos] = nums[i++];
            }
        }
        n = pos+1;
        nums.resize(n);
        return n;
    }
};
```

做个简单的修改：

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        int pos = 1,i = 1;//将pos从1开始
        while(i < n){
            if(nums[i] == nums[i-1]){
                i++;
            }else{
                nums[pos++] = nums[i++];//这里保持一致
            }
        }
        n = pos;
        nums.resize(n);
        return n;
    }
};
```

**Python代码**

```
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if n == 0: 
            return 0
        pos = 1;i = 1;count=0
        while i < n:
            if nums[i] == nums[i-1]:
                i=i+1
                count=count+1
            else:
                nums[pos] = nums[i]
                pos = pos+1
                i = i + 1
        for i in range(count):
            nums.pop(-1)
        return len(nums)
```


</article>
