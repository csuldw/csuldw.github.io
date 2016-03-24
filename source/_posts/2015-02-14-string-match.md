---
layout: post
date: 2015-02-14 12:12
title: "腾讯笔试题-字符串匹配"
tag:
	- 字符串匹配
	- 数据结构
	- 笔试
categories: 算法与数据结构
---

## 题目

假设两个字符串中所含有的字符和个数都相同我们就叫这两个字符串匹配，比如：abcda和adabc,由于出现的字符个数都是相同，只是顺序不同，所以这两个字符串是匹配的。要求高效。

<!-- more -->

## 思路

假定字符串中都是ASCII字符。用一个数组来计数，前者加，后者减，全部为0则匹配。

## 代码


```
/*---------------------------------------------
*   date：2015-02-14
*   author：LDW
*   title: 字符串匹配
*   From：腾讯
*   Blog：http://csuldw.github.io
-----------------------------------------------*/
#include <iostream>
#include <cstring>
using namespace std;

class Solution {
public:
    bool StrMatch(string str1,string str2){
        int size1 = str1.size();
        int size2 = str2.size();
        if(size1 <= 0 || size2 <= 0 || size1 != size2){
            return false;
        }
        int count[256];
        // 初始化数组
        memset(count,0,sizeof(count));
        // 前者加
        for(int i = 0; i < size1; ++i){
            ++count[str1[i]];
        }
        //后者减
        for(int i = 0; i < size2; ++i){
            --count[str2[i]];
        }
        //全部为0则匹配
        for(int i = 0; i < 256; ++i){
            if(count[i] != 0){
                return false;
            }
        }
        return true;
    }
};


int main() {
    Solution solution;
    string str1("afafafa");
    string str2("afafaf");
    cout<<solution.StrMatch(str1,str2)<<endl;
}
```