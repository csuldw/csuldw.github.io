---
title: Leetcode[263]-Ugly Number++
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/ugly-number/

Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note that 1 is typically treated as an ugly number.

---

```
class Solution {
public:
    bool isUgly(int num) {
        if (num == 0) return false;
        if (num == 1) return true;

        int pfactor[] = { 2, 3, 5 };
        for (auto val : pfactor) {
            while (num % val == 0) { num /= val; }
        }

        return num == 1;
    }
};
```


</article>
