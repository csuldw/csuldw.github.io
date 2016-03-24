---
title: Leetcode[92]-Reverse Linked List II
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/reverse-linked-list-ii/

Reverse a linked list from position m to n. Do it in-place and in one-pass.

For example:
Given 1->2->3->4->5->NULL, m = 2 and n = 4,

return 1->4->3->2->5->NULL.

Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.

----------------

**思路**：分别找到m节点和m的前节点，n节点和n的后节点，然后翻转m到n的部分，最后链接三部分成一个整体；代码如下

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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        
        if(head == NULL) return head;

        ListNode *newList = new ListNode(-1);
        newList->next = head;

        ListNode *prebegin = newList;
        ListNode *begin = head;

        ListNode *end = newList;
        ListNode *postend = head;
        
        //begin as center start node
        while(--m){
            prebegin = prebegin->next;
            begin = begin->next;
        }
        //end as center end node
        while(n--){
            end = end->next;
            postend = postend->next;
        }
        
        //reverse center part
        reverse(begin, end);
        //link three part as one list
        prebegin->next = begin;
        end->next = postend;
        
        head = newList->next;
        return head;
    }
    
    void reverse(ListNode*& begin, ListNode*& end){
        if(begin == end)
            return;
        if(begin->next == end){
            end->next = begin;
        } else {
            //at least 3 nodes
            ListNode* pre = begin;
            ListNode* cur = pre->next;
            ListNode* post = cur->next;
            while(post != end->next){
                cur->next = pre;
                pre = cur;
                cur = post;
                post = post->next;
            }
            cur->next = pre;
        }
        //swap begin and end
        ListNode* temp = begin;
        begin = end;
        end = temp;
    }
};
```


</article>
