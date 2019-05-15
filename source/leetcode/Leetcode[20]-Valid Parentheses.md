---
title: Leetcode[20]-Valid Parentheses
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/valid-parentheses/

Given a string containing just the characters `'(', ')'`, `'{', '}'`, `'[' and ']'`, determine if the input string is valid.

The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

---------------

思路：借助vector容器，存放字符'(','{','['，从左到右的读取字符串的每一个字符，

- 如果字符为上面给定的三个，则加入到vector中；
- 如果不是，在确保vector容器有字符的情况下，则让其和vector的最后一个比较；

	- 如果是匹配的，则将vector容器的大小缩减为size()-1；
	- 如果不匹配，则返回false；

- 最后，如果vector元素全部匹配完了，则返回true，否则返回false；

代码如下（c++）：

```
class Solution {
public:
    bool isValid(string s) {
        vector<char> str;
        int len = s.length();
    
        for(int i=0; i<len; i++){
            if(isThose(s[i])) {
                str.push_back(s[i]);
                continue;
            }
            if(str.size()>0 && isTrue(str[str.size()-1],s[i])) {
                cout<<str.size()<<"f"<<isTrue(str[str.size()-1],s[i])<<endl;
                str.resize(str.size()-1);
            }else{
                return false;
            }
        }
        if(str.size() == 0)
            return true;
        else
            return false;
    }
    
    bool isThose(char &a){
        if(a == '{' || a == '(' || a == '[')return true;
        return false;
    }
    
    bool isTrue(char &a, char &b){
        if( a == '(' && b == ')' ) return true;
        else if( a == '[' && b == ']' ) return true;
        else if( a == '{' && b == '}' ) return true;
        
        return false;
    }
};
```


</article>
