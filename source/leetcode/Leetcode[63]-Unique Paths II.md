---
title: Leetcode[63]-Unique Paths II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/unique-paths-ii/

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

	[
	  [0,0,0],
	  [0,1,0],
	  [0,0,0]
	]

The total number of unique paths is 2.

题意：在上题的基础上增加了点限制条件，不过大体上还是差不多的，还是采用动态规划思想，找到初始值和递推关系，和上体相比较，只是初始化和到达dp[i][j]时要加点判断语句，

- 第0列，从上到下，如果碰到一个1，则设置该位置和后面的位置都不可到达，即dp[i to n][0]；
- 第0行，从左到右，如果碰到一个1，则设置该位置和右方的位置都不可到达，即dp[0][i to n];
- 其它位置，如果该位置不为1，则到达该位置的方式共有 dp[i][j] = dp[i-1][j] + dp[i][j-1],如果为1，则dp[i][j] = 0；

代码如下：

Code(c++):

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        
        int m = obstacleGrid.size(),n = obstacleGrid[0].size();
        vector<vector<int> > dp(m, vector<int>(n));
        
        bool flag = false;
        for(int i = 0; i < m; i++) {
            if(obstacleGrid[i][0] == 1) flag = true;
            
            if(flag == false )dp[i][0] = 1;
            else dp[i][0] = 0;
        }  
        
        flag = false;
        for(int j = 0; j < n; j++) {
            if(obstacleGrid[0][j] == 1) flag = true;
            if(flag == false )dp[0][j] = 1;
            else dp[0][j] = 0;
        }    
        
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++){
                if(obstacleGrid[i][j] == 1) obstacleGrid[i][j] = 0; 
                else dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[m-1][n-1];
    }
};
```


</article>
