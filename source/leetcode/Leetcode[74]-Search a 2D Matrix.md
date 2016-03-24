---
title: Leetcode[74]-Search a 2D Matrix
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/search-a-2d-matrix/

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,

Consider the following matrix:
```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
Given target = 3, return true.

-----

这道题之前做过：<a href="http://blog.csdn.net/dream_angel_z/article/details/46413705">查找特殊矩阵中的一个数</a>

算法思路：

起始从右上角开始查找，a[i][j]初试值为a[0][n-1]，循环下列
while( i < n && j >= 0) 
如果key < a[i][j],往左走，j–，
如果key > a[i][j],则往下走，执行i++
如果key == a[i][j],表示找到了

C++代码：

```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        
        int m = matrix.size(), n = matrix[0].size();
        
        int i=0,j = n-1;
        while( i < m && j >= 0){
            if( target < matrix[i][j] ) j--;
            else if( target > matrix[i][j] ) i++;
            else return true;
        }
        return false;
    }
};
```


</article>
