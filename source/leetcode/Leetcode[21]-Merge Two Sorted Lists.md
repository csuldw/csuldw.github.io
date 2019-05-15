---
title: Leetcode[21]-Merge Two Sorted Lists
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/merge-two-sorted-lists/

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.


**思路**：要求合并两个排好序的链表。开始我们初始化头front和尾tail，然后从两个单链表的头部比较两个单链表，两链表同时不为空的条件下递归比较：

- 如果l1的值大于l2的值，就将尾部指向l1，并同步向右移动l1的头指针和tail指针
- 如果l1的值小于l2的值，就将尾部指向l2，并同步向右移动l2的头指针和tail指针

接着，如果l1不为空，则将尾指针的下一个指向l1，如果l2不为空，则将尾指针的下一个指向l2，然后将front右移一位，

最后返回front即可。

Code(c++):

```
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1;
        
        ListNode* front = new ListNode(-1);
        ListNode* tail = front;
        
        while( l1 && l2){
            if(l1->val < l2->val){
                tail->next = l1;
                l1 = l1->next;
                tail = tail->next;
            }else {
                tail->next = l2;
                l2 = l2->next;
                tail = tail->next;
            }
        }
        if(l1){
            tail->next = l1;
        }
        if(l2){
            tail->next = l2;
        }
        front = front->next;
        return front;
        
    }
};
```


</article>
