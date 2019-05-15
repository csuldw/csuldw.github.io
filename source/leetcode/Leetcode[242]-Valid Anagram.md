---
title: Leetcode[242]-Valid Anagram
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/valid-anagram/

Given two strings s and t, write a function to determine if t is an anagram of s.

For example,
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.


Note:
You may assume the string contains only lowercase alphabets.

---

思路：这道题，思路比较简单，将s中字符串的单词个数映射到26个子母中，然后遍历t，出现一个字母，就将该字母数减1，最后判断是否全为0即可。

Python代码比较简单，就给出Python代码吧：

Python：

```
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
        word = [0]*26
        for i in range(len(s)):
            index1 = ord(s[i])-97
            index2 = ord(t[i])-97
            word[index1] += 1
            word[index2] -= 1
        return not any(word)
```

附加C++:

```
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length()!=t.length())return false;
        int word[26];
        memset(word, 0, sizeof(word));
        for(int i=0;i<s.length();i++){
            int index1 = s[i]-'a';
            int index2 = t[i]-'a';
            word[index1]+=1;
            word[index2]-=1;
        }
        int c=0;
        while(c<26){
            if(word[c++]!=0)return false;
        }
        return true;
    }
};
```


学习心得：有关字符串的题目，可以考虑将其映射到字母表中。


</article>
