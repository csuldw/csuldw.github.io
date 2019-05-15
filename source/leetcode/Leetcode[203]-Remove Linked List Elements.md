---
title: Leetcode[203]-Remove Linked List Elements
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Remove all elements from a linked list of integers that have value val.

Example
Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
Return: 1 --> 2 --> 3 --> 4 --> 5

Credits:
Special thanks to @mithmatt for adding this problem and creating all test cases.

------

**分析：**

- 如果链表不为空，保证第一个节点不等于val，如果等于，直接跳到下一个节点；
- 如果此时链表为空，返回该链表；
- 将头结点赋值给一个临时节点，如果该节点的下一个节点不为空，递归遍历；

	- 如果下一个节点的值等于给定值，直接跳到下下个节点；
	- 如果下一个节点的值不等于给定值，则让跳到下个节点再来循环；

- 最后返回链表的头结点即可。

Code（c++）：

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
    ListNode* removeElements(ListNode* head, int val) {
        while(head !=NULL && head->val == val) {
            head = head->next;
        }
        if(head == NULL) return head;
        ListNode* pre = NULL;
        pre = head;
        while(pre->next!=NULL){
            if(pre->next->val == val){
                pre->next = pre->next->next;
            } else{
                pre = pre->next;
            }
        }
        return head;
    }
};
```


</article>
