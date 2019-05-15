---
title: Leetcode[119]-Pascal's Triangle II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/pascals-triangle-ii/

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].


Note:
Could you optimize your algorithm to use only O(k) extra space?

-------

分析：通过递归设置vector的值，变量i表示当前行数，同时根据行数可以得到当前的vector元素个数。如果我们从前往后遍历，当i增加的时候，我们的num[j] = num[j] + num[j-1]就会出问题，因为num[j+1]=num[j+1]+num[j],而num[j]已经更新了。

所以这里采用的是从后往前遍历，num[j] = num[j] + num[j-1],这样num[j-1] = num[j-1] + num[j-2]，不会受到前面的num[j]的变化而变化。

```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> nums(rowIndex+1);
        for(int i = 0; i <= rowIndex; i++){
            nums[i] =1;
            for(int j = i-1; j >= 1; j--){
                nums[j] = nums[j] + nums[j-1];
            }
        }
        return nums;
    }
};
```

**Python代码**

```
class Solution(object):
    def getRow(self, rowIndex):
        """
        :type rowIndex: int
        :rtype: List[int]
        """
        if rowIndex < 0: return None
        nums = []
        for i in range(rowIndex+1):
            a = []
            for j in range(i+1):
                if j==0:
                    a.append(1)
                else:
                    if j < i:
                        a.append(nums[i-1][j-1]+nums[i-1][j])
                    else:
                        a.append(1)
            nums.append(a)
        return nums[-1]
```


</article>
