---
title: Leetcode[292]-Nim Game
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/nim-game/


You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

---

分析：经过分析之后发现，有以下规律：

- 如果n小于4，返回true；
- 如果n%4==0，返回false；
- 否则，返回true。

C++:

```
class Solution {
public:
    bool canWinNim(int n) {
        if(n<=3) return true;
        if(n%4==0) return false;
        return true;
    }
};
```

Python：

```
class Solution(object):
    def canWinNim(self, n):
        """
        :type n: int
        :rtype: bool
        """
        if n<4:
            return True;
        if n%4==0:
            return False;
        return True;
```


</article>
