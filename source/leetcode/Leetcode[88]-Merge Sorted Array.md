---
title: Leetcode[88]-Merge Sorted Array
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/merge-sorted-array/

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

-------
分析：首先将nums1数组容量扩大到m+n,然后进行m+n-1次迭代，从后往前比较nums1和nums2的大小。

- 如果nums1的值大于nums2的值，就将nums1的值放到nums1的最后一个；
- 如果nums2的值大于nums1的值，就将nums2的值放到nums1的最后一个；
- ......
- 依次迭代
- ......

最后会必定有一个数组放完了，然后判断，哪个数组没放完，就接着放。

Code(c++):

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        nums1.resize(m+n);
        int j = m-1, k = n-1;
        for(int i = m+n-1; i >= 0; --i) {
            if(j >=0 && k >= 0){
                if(nums1[j] >= nums2[k]){
                    nums1[i] = nums1[j--];
                }else if(nums1[j] < nums2[k]){
                    nums1[i] = nums2[k--];
                }
            }else if(k>=0){
                nums1[i] = nums2[k--];
            }else if(j>=0){
                nums1[i] = nums1[j--];
            }
        }
    }
};
```

方法二：

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int s = m+n-1;
        nums1.resize(m+n);
        int i = m-1,j = n-1;
        while(i>=0 && j>=0){
            if (nums1[i] >= nums2[j]){
                nums1[s--] = nums1[i--];
            }else if(nums1[i] <nums2[j]){
                nums1[s--] = nums2[j--];
            }
        }  
        
        while(j>=0){
            nums1[s--] = nums2[j--];
        }
        while(i>=0){
            nums1[s--] = nums1[i--];
        }
    }
};
```


</article>
