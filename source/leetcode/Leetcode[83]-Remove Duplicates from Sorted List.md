---
title: Leetcode[83]-Remove Duplicates from Sorted List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/remove-duplicates-from-sorted-list/

Given a sorted linked list, delete all duplicates such that each element appear only once.

For example,

	Given 1->1->2, return 1->2.
	Given 1->1->2->3->3, return 1->2->3.

-----

比较简单，直接贴代码了！

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
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode * pre = head;
        
        while(pre && pre->next){
            if(pre->val == pre->next->val){
                pre->next = pre->next->next;
                continue;
            }
            pre = pre->next;            
        }
        return head;
    }
};
```

**注意while循环的时候也要判断pre->next是否为空，如果为空就不用做判断了！**


</article>
