---
title: Leetcode[141]-Linked List Cycle
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link:https://leetcode.com/problems/linked-list-cycle/

Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?




-------


**分析：**设置两个临时指针，一个一次走一步，一个一次走两步，如果再次相遇，表示有环。


**Code(c++):**

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
    bool hasCycle(ListNode *head) {
        if(head==NULL || head->next==NULL) return false;
        
        ListNode *first = head,*second = head;
        while(second!=NULL && second->next!=NULL){
            second = second->next->next;  
            first = first->next;
            if(first == second)
                return true;
        }
        return false;
    }
};
```


</article>
