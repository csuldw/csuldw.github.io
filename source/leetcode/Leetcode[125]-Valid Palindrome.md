---
title: Leetcode[125]-Valid Palindrome
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/valid-palindrome/

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

```
For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.
```

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.


-----

 思路：定义两个标示符，一个指向字符串前面，一个指向字符串末尾，如果前后位置的字符不是字母或是数字，则直接跳过该字符，如果是大写，则转换成小写再比较，如果碰到不匹配的直接返回false。


Code(C++):

```
class Solution {
public:

    bool isPalindrome(string s) {
        
        int length = s.length();
        
        if(length == 0){  
            return true;  
        }  
        int i = 0, j = length-1;
        
        while(i <= j){
            if(!isStr(s[i])) i++;
            else if(!isStr(s[j])) j--;
            else if(s[i++] != s[j--]) return false;
        }
        return true;
    }
    
    
    bool isStr(char &a){
        if(a >= '0' && a <= '9' ) {
            return true;
        }else if(a >= 'a' && a <= 'z' ) {
            a -= 32;
            return true;
        } else if(a >= 'A' && a <= 'Z' ) {
            return true;
        } 
        return false;
    }
   
};

```


</article>
