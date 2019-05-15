---
title: Leetcode[202]-Happy Number
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/happy-number/

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.


Example: 19 is a happy number

$1^2 + 9^2 = 82$
$8^2 + 2^2 = 68$
$6^2 + 8^2 = 100$
$1^2 + 0^2 + 0^2 = 1$

Credits:
Special thanks to @mithmatt and @ts for adding this problem and creating all test cases.

------

**分析**：题目说的是对任意一个正整数，不断各个数位上数字的平方和，若最终收敛为1，则该数字为happy number，否则程序可能从某个数开始陷入循环。

这里我们使用一个哈希map表存储已经出现过的数字，如果下次还出现，则返回false。

```
class Solution {
public:
    bool isHappy(int n) {

        if(n==1) return true;
        map<int,int> nums;

        while(n>0){
            int number = 0;
            # 计算一个数各个位的平方和
            while(n){
                number += (n % 10) * (n % 10);
                n/=10;
            }

            if(number == 1)
                return true;
            else if(nums.find(number)!=nums.end())
                return false;
            n = number;
            nums[n] = 1;
        }
    }
};
```

另外还有一种简单的算法：

--------
```
class Solution {
public:
    bool isHappy(int n) {
        while(n>6){  
            int next = 0;  
            while(n){
                next+=(n%10)*(n%10); 
                n/=10;
            }  
            n = next;  
        }  
        return n==1;  
    }
};
```


</article>
