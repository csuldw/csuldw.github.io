---
layout: post
date: 2016-09-14 18:24
title: "Dynamic Programming Example：Maximum Sum Submatrix in Matrix"
categories: 算法与数据结构
tag: 
	- 动态规划
	- DP
	- 最大子矩阵和
	- 算法
comment: true
---

## 问题描述

> Find maximum sum submatrix in a given 2D matrix of integers.

<!-- more -->

**输入**：

- 第1组`n m`：表示一个数组是$n$行$m$列的；
- 第2组：输入第一个$n$行$m$列的数组；

**返回**：

- 最大子矩阵和

**样例：**

给定一个$n * m$维的二维数据，如下

<pre><code class="markdown">4	4
0	-2	-7	0
9	2	-6	2
-4	1	-4	1
-1	8	0	-2
</code></pre>

最后输出子矩阵和：

<pre><code class="markdown">15
</code></pre>

## 最大连续子序列和

最大子矩阵与[最大连续子序列和](http://www.geeksforgeeks.org/largest-sum-contiguous-subarray/)有着千丝万缕的联系，最大连续子序列的DP动态转移方程为

$$dp[i] = max(dp[i-1] + arr[i], arr[i])$$

其中$dp[i]$表示从左到右到第$i$个元素时的最大子序列和， $arr$ 是子序列，这里可以使用一维数组表示。代码如下：

```
int maxSubArray(vector<int> arr){
    int n = arr.size();
    vector<int> dp(n);
    int maxVal = INT_MIN;
    for(int i = 0; i < n; ++i){
        dp[i] = i == 0 ? arr[i] : max(arr[i], dp[i - 1] + arr[i]);
        maxVal = maxVal > dp[i] ? maxVal : dp[i];
    }
    return maxVal;
}
```

通过设置一个最大和变量`maxVal`，每次获取$dp[i]$时就将两者比较一下，并将大的赋值给`maxVal`即可。


## 最大子矩阵和

### Step1: 构造行和矩阵 

首先开辟一个临时矩阵`sumMatrix`，第$i$行各列的元素表示的是原始矩阵$matrix$第$0$行到第$i$行各列的元素之和，使用公式表示就是：

$$sumMatrix[i][j] = \sum\_{k = 0}^i matrix[k][j]$$

可以将原始矩阵转为行和矩阵：

$$
\begin{bmatrix}
 a\_{11} &a\_{12}  &\cdots & a\_{1m} \\\\ 
 a\_{21} &a\_{22}  &\cdots & a\_{2m} \\\\ 
 \vdots  &\vdots  & \cdots  & \vdots  \\\\ 
 a\_{n1} &a\_{n2}  &\cdots & a\_{nm} \\\\ 
\end{bmatrix}
\Rightarrow 
\begin{bmatrix}
 a\_{11} &a\_{12}  &\cdots & a\_{1m} \\\\ 
  a\_{11} + a\_{21} & a\_{12} + a\_{22}  &\cdots &  a\_{1m} + a\_{2m} \\\\ 
 \vdots  &\vdots  & \cdots  & \vdots  \\\\ 
  \sum\_{i=1}^n a\_{i1} &\sum\_{i=1}^n a\_{i2}  &\cdots & \sum\_{i=1}^n a\_{im} \\\\ 
\end{bmatrix}
$$

### Step2：寻找状态转移矩阵

变量说明：

- matrix: 原始矩阵
- sumMatrix: 行和矩阵

#### 解释一

分析下，如果最终得到的是一维子数组，那么有两种情况，第一种是行子数组，第二种是列子数组，如果是行子数组，则相当于在原数组matrix上对每行执行一次**最大连续子序列和**方法并取最大的值即可，如果切换到行和矩阵上，则原始数据matrix的第$i$行等价于行和矩阵sumMatrix的第$i$行减去$i-1$行的值，即$sumMatrix[i][j] - sumMatrix[i - 1][j]$；同理，如果是列子数组，假设是第$i$行到第$j$行的列子数组，则等价到行和数组上，就是第$j$行每一列的值减去第$i$行每一列的值，然后求解此时最大的一列即可。那么，如果是多行多列的子数组呢？

对于这种情况，可以设置一个变量$k$，用以调整子矩阵的行数，当$k$等于0的时候，表示的是一维数组，也就是上面讨论的一维行子矩阵的情况；当$k$等于$1$的时候，则表示子矩阵的行数为$2$，那么通过什么来获取这个子矩阵呢？我们可以根据行和矩阵，以间隔为$1$进行相减获取子矩阵的列和并将其转为一维数组，即$sumMatrix[i][j] - sumMatrix[i - 1 - 1][j]$；同理，当$k$等于$2,3,\cdots$的时候，子矩阵的列和就为$sumMatrix[i][j] - sumMatrix[i - 1 - k ][j]$，然后求解此时的一维数组的最大子序列和，需要注意的是，当$i == k$时，由于是$k+1$行相加，因此首行就是$sumMatrix[i][j]$。 因此可以得到下列递推式式子：

```
temp[j] = k == 0 ? matrix[i][j] : i == k ? sumMatrix[i][j] : sumMatrix[i][j] - sumMatrix[i - k - 1][j]
```

### 解释二

同样设置一个变量$k$，并且此时的$k$同样代表子矩阵的行数，但规则不一样。当$k=0$时，我们根据行号$i$来确定子矩阵的行数，如果是第$i$行，则表示子矩阵的行数也是$i$取值为前$i$行,也就是行和矩阵$sumMatrix[i][j]$；当$k=1$时，则当行号为$i$时，我们取(k, i]行之间的元素，即$sumMatrix[i][j] - sumMatrix[i - k][j]$。因此可以得到下列状态转移方程：

```
temp[j] = k == 0 ? sumMatrix[i][j] : sumMatrix[i][j] - sumMatrix[i - k][j]
```

### 解释三

同样设置一个变量$k$，此$k$用以表示子矩阵的首行，由于子矩阵肯定是连续的行，因此，当$k=0$时，根据行号$i$依次得到子矩阵的行和$[0, i]$；当$k=1$时，表示子矩阵的首行为$1$，根据$i$依次得到$[k, i]$行的元素；由此可以得出状态转移矩阵为：

```
temp[j] = k == 0 ? sumMatrix[i][j] : sumMatrix[i][j] - sumMatrix[k-1][j];
```

当得到这个递推式子之后，每次得到新的子矩阵列和，然后根据新的一维数组求解最大连续子序列。


上面给出了三种解释，因此可以根据之前的推导编写出下列代码，源代码如下：


```
#include<iostream>
#include<vector>
using namespace std;
int maxSubArray(vector<int> arr);
int maximumSubMatrix(vector<vector<int> > matrix);
int main(){
    int n, m;
    while(cin>>n>>m){
        vector<vector<int> > matrix;
        for(int i = 0; i < n; i++){
            vector<int> temp;
            for(int j = 0; j < m; ++j){
                int value;
                cin>>value;
                temp.push_back(value);
            }
            matrix.push_back(temp);
        }
        cout<<maximumSubMatrix(matrix)<<endl;
    }
    return 0;
}

int maxSubArray(vector<int> arr){
    int n = arr.size();
    vector<int> dp(n);
    int maxVal = INT_MIN;
    for(int i = 0; i < n; ++i){
        dp[i] = i == 0 ? arr[i] : max(arr[i], dp[i - 1] + arr[i]);
        maxVal = maxVal > dp[i] ? maxVal : dp[i];
    }
    return maxVal;
}

int maximumSubMatrix(vector<vector<int> > matrix){
    int n = matrix.size();
    int m = matrix[0].size();
    vector<vector<int> > sumMatrix(n, vector<int>(m)), dp(n, vector<int>(m));
    int i, j, k;
    for(i = 0; i < n; ++i){
        for(j = 0; j < m; ++j){
           sumMatrix[i][j] =  i == 0 ? matrix[i][j] : sumMatrix[i-1][j] + matrix[i][j];
        }
    }
    int maxVal = INT_MIN;
    vector<int> temp(m);
    for(k = 0; k < n; ++k){
        for(i = k; i < n; ++i){
            for(j = 0; j < m; ++j){
                //temp[j] = k == 0 ? matrix[i][j] : i == k ? sumMatrix[i][j] : sumMatrix[i][j] - sumMatrix[i - k - 1][j];
                temp[j] = k == 0 ? sumMatrix[i][j] : sumMatrix[i][j] - sumMatrix[i - k][j];
                //temp[j] = k == 0 ? sumMatrix[i][j] : sumMatrix[i][j] - sumMatrix[k - 1][j];
            }
            int arrVal = maxSubArray(temp);
            maxVal = max(maxVal, arrVal);
        }
    }
    return maxVal;
}
```


关于DP，有个博客讲解的非常详实，感兴趣的可以看看，地址：[演算法笔记](http://www.csie.ntnu.edu.tw/~u91029/DynamicProgramming.html)。


## References

- [Chapter_07_-_Submatrix_with_largest_sum](https://prismoskills.appspot.com/lessons/Dynamic_Programming/Chapter_07_-_Submatrix_with_largest_sum.jsp)
- [Maximum Sum Rectangular Submatrix in Matrix dynamic programming/2D kadane](https://www.youtube.com/watch?v=yCQN096CwWM)