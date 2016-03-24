---
title: Leetcode[118]-Pascal's Triangle
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/pascals-triangle/

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
---------
分析：

- 第j=0列全为1，第j==i列时，都为1
- 其它列
	- a[2][1] = a[1][0]+a[1][1]
	- a[3][1] = a[2][0]+a[2][1]
	- a[3][2] = a[2][1]+a[2][2]
	- ......
	- 推算得出
	- ......
	- a[i][j] = a[i-1][j-1]+a[i-1][j]

**代码（c++）：**

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector <vector<int> > vec(numRows);
        for(int i = 0; i < numRows; i++) {
            vec[i].resize(i+1);
            for(int j = 0; j <= i ; j++){
                if(j==i || j==0)
                    vec[i][j] = 1;
                else{
                    vec[i][j] = vec[i-1][j-1] + vec[i-1][j];
                }
            }
        }
        return vec;
    }
};
```

**代码（Python）：**

```
class Solution(object):
    def generate(self, numRows):
        """
        :type numRows: int
        :rtype: List[List[int]]
        """
        b=[]
        for i in range(numRows):
            temp = []
            for j in range(i+1):
                if j==0:
                    temp.append(1)
                else:
                    if j < i:
                        temp.append(b[i-1][j-1]+b[i-1][j])
                    elif j==i:
                        temp.append(1)
            b.append(temp)
        return b
```


</article>
