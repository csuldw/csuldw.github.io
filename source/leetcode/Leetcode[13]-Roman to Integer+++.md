---
title: Leetcode[13]-Roman to Integer+++
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/roman-to-integer/


Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.


---

分析：

从前往后扫描，用一个临时变量记录分段数字。

如果当前处理的字符对应的值和上一个字符一样，那么临时变量加上这个字符。比如III = 3
如果当前比前一个大，说明这一段的值应该是当前这个值减去前面记录下的临时变量中的值。比如IIV = 5 – 2
如果当前比前一个小，那么就可以先将临时变量的值加到结果中，然后开始下一段记录。比如VI = 5 + 1


C++:

```
class Solution {
public:
    int romanToInt(string s) {
        int num[256] = { 0 };
        num['I'] = 1; num['V'] = 5; num['X'] = 10; num['L']=50;
        num['C'] = 100; num['D'] = 500; num['M'] = 1000;
        int result = 0;
        int i = 0;
        while (i < s.size()){
            if (num[s[i]] < num[s[i+1]])
                result -= num[s[i]];
            else
                result += num[s[i]];
            i++;
        }
        return result;
    }
};
```






Python：

```
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        Num = {"I":1, "V":5, "X":10, "L":50, "C":100, "D":500, "M":1000}
        L = len(s)
        sum = 0
        i = 0
    
        while(i < L - 1):
    
            if(Num.get(s[i]) < Num.get(s[i + 1])):
                sum -= Num.get(s[i])
            else:
                sum += Num.get(s[i])
            i += 1
        sum += Num.get(s[i])
        return sum
```


</article>
