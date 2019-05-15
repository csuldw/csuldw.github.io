---
title: Leetcode[215]-Kth Largest Element in an Array
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/kth-largest-element-in-an-array/

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given`[3,2,1,5,6,4] `and `k = 2`, return `5`.

Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.

Credits:
Special thanks to @mithmatt for adding this problem and creating all test cases.


------

法一：使用STL的sort排序O(NlogN)

```
int findKthLargest(vector<int>& nums, int k) {
    sort(nums.begin(),nums.end());
    return nums[nums.size()-k];
}
```

法二：自己写快速排序O（NlogN）

```
int findKthLargest(vector<int>& nums, int k){
   
	quickSort(nums, 0 ,nums.size());
	return nums[nums.size()-k];
   
}
void quickSort(vector<int> &nums, int left, int right){
    int i = left, j = right - 1;
    if(i < j){
        int po = nums[i];
        while(i < j){
            while(i < j && nums[j] >= po) j--;
            if(i < j) {
                nums[i++] = nums[j];
            }
            while(i < j && nums[i] <= po ) i++;
            if(i < j){
                nums[j--] = nums[i];
            }
        }
        nums[i] = po;
        quickSort(nums, left, i);
        quickSort(nums, i+1, right);
        
    }
}
```

法三：使用建堆法  时间复杂度O(klogN)

```
int findKthLargest(vector<int>& nums, int k){
    make_heap(nums.begin(), nums.end());
    for(auto i=0; i<k-1;i++){
        pop_heap(nums.begin(), nums.end());
        nums.pop_back();
    }
    return nums.front();
}
```

方法四：此方法是论坛看到的，O（N）

```
int findKthLargest(vector<int>& nums, int k){
     int i, m, n, pivot, head =0, tail = nums.size()-1, maxV;

    while(1){
        m = head, n= tail;
        pivot = nums[m++];
        while(m <= n) {
            if(nums[m] >= pivot) m++;
            else if(nums[n] < pivot) n--;
            else {
                swap(nums[m++], nums[n--]);
            }
        }
        if(m-head == k) 
            return pivot;
        else if(m-head < k) {
            k -= (m-head); 
            head = m;  
        }
        else {
            tail = m-1;
            head = head+1;
        }
    }
    
}
```


</article>
