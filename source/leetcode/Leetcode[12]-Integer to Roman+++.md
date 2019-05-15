---
title: Leetcode[12]-Integer to Roman+++
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/integer-to-roman/


Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.


---

分析：


C++:

```
class Solution {
public:
    string intToRoman(int num) {
        int s[7]={0};
        int Roman[7]={1000,500,100,50,10,5,1};
        char ch[7]={'M','D','C','L','X','V','I'};
        int index=0;
        string result;
        while(index<7) {
            s[index]=num/Roman[index];
            num%=Roman[index++];
        }
        for(int i=0;i<7;i++){
            if (s[i]==1&&i+1<7&&s[i+1]==4)
                continue;
            if(s[i]==4){
                if (s[i-1]==1)
                    result=result+ch[i]+ch[i-2];
                else
                    result=result+ch[i]+ch[i-1];
            }else{
                for(int j=0;j<s[i];j++)
                    result+=ch[i];
            }
        }
        return result;
    }
};
```


</article>
