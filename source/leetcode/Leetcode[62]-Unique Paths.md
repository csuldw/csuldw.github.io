---
title: Leetcode[62]-Unique Paths
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/unique-paths/

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![这里写图片描述](http://articles.leetcode.com/wp-content/uploads/2014/12/robot_maze.png)

Above is a 3 x 7 grid. How many possible unique paths are there?

Note: m and n will be at most 100.

---

题目：给定一个m*n的矩阵，让机器人从左上方走到右下方，只能往下和往右走，一共多少种走法。

思路：采用动态规划设计思想，到第0列和第0行的任何位置都只有1种走法，即初始化为d[0][\*] = 1,d[\*][0] = 1;当机器人走到第i行第j列的时候，它的走法总数是等于第i-1行第j列的总数加上第i行第j-1列的总数，即dp [ i ] [ j ]  = d[i-1][j] + dp[i][j-1].代码如下:


Code(c++): 

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int> > dp(m,vector<int> (n));
        for(int k = 0; k < m; k++) {
            dp[k][0] = 1;
        }
        for(int t = 0; t < n; t++) {
            dp[0][t] = 1;        
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```


</article>
