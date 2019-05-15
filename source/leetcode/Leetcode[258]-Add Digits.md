---
title: Leetcode[258]-Add Digits
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/add-digits/

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

For example:

Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.

Follow up:
Could you do it without any loop/recursion in O(1) runtime?

---

思路：此题和202题有点类似，可以参考。主要是在各个位数的想加上，有点技巧。


C++

```
class Solution {
public:
    int addDigits(int num) {
        int n=num;
        while(n>=10){
            int i = 0;
            while(n>0){
                i += n%10;
                n = n/10;
            };
            n = i;
        }
        return n;
    }
};
```


</article>
