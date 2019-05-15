---
title: Leetcode[148]-Sort List
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/sort-list/

Sort a linked list in O(n log n) time using constant space complexity.

-------


**分析：**题目要求时间复杂度为O(nlogn)，所以一开始想到的就是快速排序，但是快速排序一直AC不了，然后就想到用归并排序，没想到归并排序竟然可以。下面给出详细代码：

归并排序需要做的

- 找到中间点
- 合并两个排好序的链表
- 递归实现归并排序


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
    //mergeSort
    ListNode* sortList(ListNode* head) {
    
        if(head == NULL || head->next==NULL) return head;
    
        ListNode *mid = getMid(head);
        ListNode *left = head,*right;
    
        if(mid){
            cout<<mid->val<<endl;
            right = mid->next;
            mid->next = NULL;
        }
    
        return mergeLinkedList(sortList(left),sortList(right));
    
    }
    //get middle point from ListNode
    ListNode* getMid(ListNode* head){
        if(head==NULL || head->next==NULL ) return head;
    
        ListNode* first = head,* second = head->next;
    
        while(second && second->next){
            first = first->next;
            second = second->next->next;
        }
        return first;
    }
    
    //merge two sorted Linked List
    ListNode* mergeLinkedList(ListNode* first,ListNode* second){
        if(first==NULL) return second;
        if(second==NULL) return first;
    
        ListNode* tail,* front;
        front = new ListNode(-1);
        tail = front;
        while(first && second){
            if(first->val < second->val){
                tail->next = first;
                first = first->next;
                tail = tail->next;
            }else{
                tail->next = second;
                second = second->next;
                tail = tail->next;
            }
        }
        if(first){
            tail->next =first;
        }
        if(second){
            tail->next = second;
        }
        front = front->next;
        return front;
    }
};
```


</article>
