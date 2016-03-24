---
title: Leetcode[169]-Majority Element
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Credits:
Special thanks to @ts for adding this problem and creating all test cases.

-----
思路一：将数组排好序，中间的那个数一定就是我们需要的majority element。时间复杂度O(nlogn)

思路二：Moore voting algorithm--每找出两个不同的element，就成对删除即count--，最终剩下的一定就是所求的。时间复杂度：O(n)

Code1(C++):

```
int majorityElement1(vector<int>& nums) {
    int n = nums.size();
    sort(nums.begin(),nums.end());
    return nums[n/2];
}
```

Code2(C++)

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();  
        int count = 0,number;
        for(int i=0;i < n; i++){
            if(count == 0) {
                number = nums[i];
                count++;
            } else {
                number == nums[i] ? count++: count--;
            }
        }
        return number;
    }
};
```


</article>
