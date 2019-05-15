---
title: Leetcode[36]-Valid Sudoku
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/valid-sudoku/

Determine if a Sudoku is valid, according to: Sudoku Puzzles - The Rules.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.
![这里写图片描述](http://img.blog.csdn.net/20150609210526658)

A partially filled sudoku which is valid.

Note:
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.


-------
分析：验证一个数独的合法性，从三个方向入手

- 每一行不能出现重复元素
- 每一列不能出现重复元素
- 每一个九方格不能出现重复元素

我采用的是map来保存元素的数值，然后判断map中是否有，有则返回false；参数之间的关系推导如下：
![这里写图片描述](http://img.blog.csdn.net/20150609213752137)
</a>
代码如下

Code(c++):
```
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        //row
        for(int  i = 0; i<9; i++) {
            map<int, int> mp;
            for (int j = 0; j<9; j++){
                if(mp.find(board[i][j])!=mp.end())
                    return false;
                if(board[i][j] == '.')continue;
                mp[board[i][j]] = j;
            }
        }
        
        //cow
        for(int  j = 0; j<9; j++) {
            map<int, int> mp;
            for (int i = 0; i<9; i++){
                if(mp.find(board[i][j])!=mp.end())
                    return false;
                if(board[i][j] == '.')continue;
                mp[board[i][j]] = i;
            }
        }
        
        //board
        for(int i = 0; i < 9; i++){
            map<int,int> mp;
            for(int j = (i/3)*3; j < 3 + (i/3)*3 ; j++){
                for(int k = (3*i)%9; k <= (3*(i+1)-1)%9; k++){
                    if(mp.find(board[j][k])!=mp.end())
                        return false;
                    if(board[j][k] == '.')continue;
                    mp[board[j][k]] = k;
                }
            }
        }
        
        return true;
        
    }
};
```


</article>
