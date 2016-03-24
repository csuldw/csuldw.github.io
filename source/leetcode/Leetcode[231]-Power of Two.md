---
title: Leetcode[231]-Power of Two
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/power-of-two/

Given an integer, write a function to determine if it is a power of two.

---

分析：

- 如果n小于0，返回false；
- 如果n等于1或者等于2，返回true；
- 当n大于2时，根据n大于2的条件进行递归，，如果n%2不为0则直接返回false，否则将n/2赋值给n
	- 如果循环结束后还没有返回false，则返回true。


C++:

```
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if(n<=0) return false;
        if (n==2||n==1) return true;
        while(n>2){
            if(n%2!=0) return false;
            n/=2;
        }
        return true;
    }
};
```


</article>
