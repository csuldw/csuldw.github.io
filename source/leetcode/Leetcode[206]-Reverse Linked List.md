---
title: Leetcode[206]-Reverse Linked List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/reverse-linked-list/

Reverse a singly linked list.Reverse a singly linked list.

Hint:
A linked list can be reversed either iteratively or recursively. Could you implement both?

-------

分析：
![这里写图片描述](http://img.blog.csdn.net/20150610095309420)

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
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL;
        if(head == NULL || head->next==NULL) return head;
    
        pre= head->next;
    
        head->next = NULL;
        while(pre!=NULL){
            ListNode * nextNode = NULL;
            nextNode = pre->next;
            pre->next = head;
            head = pre;
            pre = nextNode;
            delete nextNode;
        }
        delete pre;
        return head;
    }
};
```





</article>
