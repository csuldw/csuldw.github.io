---
title: Leetcode[191]-Number of Bits
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/number-of-1-bits/

Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).

For example, the 32-bit integer ’11' has binary representation 00000000000000000000000000001011, so the function should return 3.

---

分析：**在十进制转换为二进制时，如果n%2=1，则在二进制中有1**.根据这条规则，可以循环判断n，每判断一次，n=n/2，代码如下：

C++:


```
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while(n>0){
            if(n%2==1){
                count++;
            }
            n=n/2; 
        }
        return count;
    }
};
```


Python:

```
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        count = 0
        while n>0:
            if n%2==1:
                count=count+1
            n=n/2
        return count
```


</article>
