---
title: Leetcode[19]-Remove Nth Node From End of List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/remove-nth-node-from-end-of-list/

Given a linked list, remove the nth node from the end of list and return its head.

For example,

	   Given linked list: 1->2->3->4->5, and n = 2.

	   After removing the second node from the end, the linked list becomes 1->2->3->5.
Note:
Given n will always be valid.
Try to do this in one pass.

--------

**思路：**先将链表翻转，然后遍历，找到第n-1个节点，然后删除第n个节点，最后再次翻转链表即可。

**笔记：**在链表翻转后，我采用的是在翻转后的链表的头部放一个无关的节点，然后往后面找，变量从1到n，当变量为n的时候，此时节点还处于n-1节点上，接着我们就让该节点的下个节点指向它的下下个节点，这样第k个节点就删除了。

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
    
        reverseList(head);
        ListNode *newList = new ListNode(-1);
        newList->next = head;
    
        int count = 1;
        ListNode *pre = newList;
    
        //find the prior element
			//when we get the Nth Node,delete this node from list
            if(count==n && pre->next!=NULL){
                pre->next = pre->next->next;
                break;
            }else{
                pre = pre->next;
                count++;
            }
        }
        head = newList->next;
        //when head is not null,reverse it
        if(head!=NULL)
            reverseList(head);
        return head;
    }
    
    void reverseList(ListNode* &head){
        if(head==NULL || head->next == NULL)return;
        
        ListNode *newList = new ListNode(-1);
        ListNode *pre = head;
        
        ListNode *temp;
        while(pre!=NULL){
            temp = pre->next;
            pre->next = newList->next;
            newList->next = pre;
            pre = temp;
        }
        newList = newList->next;
        head = newList;
    }
};
```


</article>
