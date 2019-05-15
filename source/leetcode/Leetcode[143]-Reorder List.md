---
title: Leetcode[143]-Reorder List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/reorder-list/

Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

You must do this in-place without altering the nodes' values.

For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.

-----

思路：将列表分成两半，然后把后一半翻转，最后一个一个构成新链表。

C++:

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

    void reorderList(ListNode* head) {
        
        if(head==NULL || head->next==NULL) return;
        
        ListNode *mid = getMid(head);
        ListNode *left = head,*right = reverseList(mid->next);
        
        mid->next = NULL;
        
        ListNode* newHead = new ListNode(-1);
        ListNode* newTail = newHead;
        while(left && right){
            ListNode *temp = left->next;
            left->next = newTail->next;
            newTail->next = left;
            newTail = newTail->next;
            left = temp;
    
            temp = right->next;
            right->next = newTail->next;
            newTail->next = right;
            newTail = newTail->next;
            right = temp;
        }
        if(left) newTail->next = left;
        if(right) newTail->next = right;
        newHead = newHead->next;
        head = newHead;
    }
    //reverse list
    ListNode* reverseList(ListNode* head){
        if(head==NULL || head->next==NULL) return head;
        
        ListNode *pre = head;
        ListNode *newList = new ListNode(-1);
        while(pre!=NULL){
            ListNode* temp = pre->next;
            pre->next = newList->next;
            newList->next = pre;
            pre = temp;
        }
        newList = newList->next;
        return newList;
    }
    //get the middle element
    ListNode* getMid(ListNode* head){
        if(head==NULL ||head->next ==NULL)return head;
        
        ListNode *first=head,*second=head->next;
        
        while(second && second->next){
            first = first->next;
            second = second->next->next;
        }
        return first;
    }
};
```


</article>
