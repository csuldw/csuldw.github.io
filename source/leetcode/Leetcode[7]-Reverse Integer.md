---
title: Leetcode[7]-Reverse Integer
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/reverse-integer/

Reverse digits of an integer.

	Example1: x = 123, return 321
	Example2: x = -123, return -321

-----


取一个数的最后一位，用`x % 10`,取一个数的前n-1位（共n位），用`x/10`，每一次取的时候，都将上一次取的数乘以10，然后再加上末尾的数即可，代码如下：

Code(C++)

```
class Solution {
public:
    int reverse(int x) {
        long result = 0;
        while(x != 0)
        {
            result = result*10 + x % 10;
            x /= 10;
        }
        return (result > INT_MAX || result < INT_MIN)? 0 : result;
    }
    
};

```


</article>
