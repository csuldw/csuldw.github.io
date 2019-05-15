---
title: Leetcode[9]-Palindrome Number
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/palindrome-number/

Determine whether an integer is a palindrome. Do this without extra space.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

-------------

思路：求出它的反转数，然后比较两个数是否恒等于即可。

C++代码：


```
class Solution {
public:
    bool isPalindrome(int x) {
        
        if(x < 0) return false;
        int y = x, result = 0;
        while( y > 0 ){
            result = result * 10 + y%10;
            y /= 10;
        }
        return result == x;
    }
};

```

法二：将数字转换成字符串，遍历一遍字符串，看首位是否相等即可

```
    bool isPalindrome(int x) {
        
        if(x < 0) return false;
        if(x>=0 && x<=9) return true;
        string str;
        stringstream ss;
        ss<<x;
        ss>>str;
        int len = str.length();
        for(int i = 0; i < (len/2); i++ ){
            if(str[i] != str[len-1-i])return false;
        }
        return true;
        
    }
```


</article>
