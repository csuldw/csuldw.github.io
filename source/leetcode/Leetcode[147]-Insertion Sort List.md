---
title: Leetcode[147]-Insertion Sort List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/insertion-sort-list/

Sort a linked list using insertion sort.

----
链表的插入排序

思路：新开辟一个链表空间，用来作为插入排序的目标链。循环遍历原链表，对每个节点，让其从头到尾的在已经排好序的链表中找到插入位置（此处记录的是插入位置的前一个位置），然后将其插入进去即可。

代码Code(c++)：

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
    ListNode* insertionSortList(ListNode* head) {
        
        if(head == NULL || head->next ==NULL) return head;
        ListNode *pre = new ListNode(-1);
    
        pre->next = head;
        head = head->next;
        pre->next->next = NULL;
        ListNode *temp,*headNext;
    
        while(head!=NULL){
            ListNode *prior = pre;
            temp = pre->next;
            while(temp){
                if(head->val > temp->val){
                    prior = temp;
                    temp = temp->next;
                }
                else break;
            }
            headNext = head->next;
            head->next = prior->next;
            prior->next = head;
            head = headNext;
        }
        head= pre->next;
        return head;
    }
};
```


</article>
