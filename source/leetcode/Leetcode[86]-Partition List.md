---
title: Leetcode[86]-Partition List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link： https://leetcode.com/problems/partition-list/

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

For example,
Given 1->4->3->2->5->2 and x = 3,
return 1->2->2->4->3->5.

-------

思路：设置两个临时单链表，一个存放小于x的链表，一个存放大于x的链表，然后遍历head，一次存入两个链表中，最后合并两个链表。

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
    ListNode* partition(ListNode* head, int x) {
        if(head == NULL || head->next==NULL) return head;
        ListNode *left = new ListNode(-1),*right = new ListNode(-1);
        ListNode *ltail = left,* rtail = right;
        ListNode *pre = head;
        while(pre){
            if(pre->val < x) {
                ltail->next = pre;
                ltail = ltail->next;
            }else{
                rtail->next = pre;
                rtail = rtail->next;
            }
            pre = pre->next;
        }
        if(right->next){
            ltail->next = right->next;
            rtail->next = NULL;//set rtail as NULL, otherwise time limited error
        }
        left = left->next;
        return left;
    }
};
```


</article>
