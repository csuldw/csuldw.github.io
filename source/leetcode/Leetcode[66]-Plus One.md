---
title: Leetcode[66]-Plus One
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/plus-one/

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.


--------
题意：给定一个数组，表示的是非负数的各个位的数，现在将该数加一，求加一后得到的数组。

分析：由于加以后数组的长度可能发生变化，说以不能单纯的直接在数组后面加一。可以先将数组翻转，个位转到前面来，然后从前往后依次加一，最后判断如果在最后一位相加超过十了，就将数组长度加一，代码如下：

Code(c++):
```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();
        reverse(digits.begin(),digits.end());
        int temp = 0,flag = false;
        for(int i = 0; i<n ; i++) {
            if(i==0) temp =1;
            int sum = digits[i] + temp;
            digits[i] = sum %10;
            temp = sum/10;
            if(i == n-1 && sum >= 10)
                flag = true;
        }
        if(flag){
            digits.resize(n+1);
            digits[n] = temp;
        }
        reverse(digits.begin(),digits.end());
        return digits;
    }
};
```


</article>
