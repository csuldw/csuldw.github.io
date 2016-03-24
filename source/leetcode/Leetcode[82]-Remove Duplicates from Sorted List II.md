---
title: Leetcode[82]-Remove Duplicates from Sorted List II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given `1->2->3->3->4->4->5`, return`1->2->5`.
Given `1->1->1->2->3`, return `2->3`.

-------

思路：跟上道题<a href="http://blog.csdn.net/dream_angel_z/article/details/46446067">Leetcode[83]-Remove Duplicates from Sorted List</a>算法有点区别，这道题需要设置一个标示符，如果某一趟比较的时候两个元素相等了，就设flag等于true，接着下一趟循环如果两个元素不相等，但此时的flag为true，就需要将两个元素前面的那个删掉。

最后，必定第二个元素为空而终止，此时还需要判断flag是否为true，如果为true，则末尾的元素还需要删掉；

代码如下：
Code(c++):

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
       ListNode * newList = new ListNode(-1);
        newList->next = head;
        ListNode *pre = newList;
    
        bool flag = false;
        while(pre->next && pre->next->next){
            if(pre->next->val == pre->next->next->val){
                pre->next = pre->next->next;
                flag = true;
                continue;
            }
            if(flag == true){
                pre->next = pre->next->next;
                flag = false;
                continue;
            }
            pre = pre->next;
        }
        //最后还需要判断是否上次判断时元素重复
        if(flag == true){
            pre->next = pre->next->next;
        }
        head = newList->next;
        return head;
    }
};
```


</article>
