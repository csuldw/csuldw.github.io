---
title: "LeetCode部分题解"
layout: post
date: 2015-12-12
categories: 算法与数据结构
tag: 
	- LeetCode
	- 数据结构
comments: true
---

本文是先前做LeetCode时的部分题解，有的题目既包含C++代码，也有Python代码，为方便查阅，决定将这些思路合并到一文之中。

![leetcode](/assets/articleImg/2015-12-12-leetcode.png)

<!--more--> 

题解目录如下：

- [Leetcode[1]-Two Sum](#Leetcode[1]-Two_Sum)
- [Leetcode[4]-Median of Two Sorted Arrays](#Leetcode[4]-Median_of_Two_Sorted_Arrays)
- [Leetcode[7]-Reverse Integer](#Leetcode[7]-Reverse_Integer)
- [Leetcode[9]-Palindrome Number](#Leetcode[9]-Palindrome_Number)
- [Leetcode[12]-Integer to Roman+++](#Leetcode[12]-Integer_to_Roman+++)
- [Leetcode[13]-Roman to Integer+++](#Leetcode[13]-Roman_to_Integer+++)
- [Leetcode[15]-3Sum](#Leetcode[15]-3Sum)
- [Leetcode[18]-4Sum](#Leetcode[18]-4Sum)
- [Leetcode[19]-Remove Nth Node From End of List](#Leetcode[19]-Remove_Nth_Node_From_End_of_List)
- [Leetcode[20]-Valid Parentheses](#Leetcode[20]-Valid_Parentheses)
- [Leetcode[21]-Merge Two Sorted Lists](#Leetcode[21]-Merge_Two_Sorted_Lists)
- [Leetcode[26]-Remove Duplicates from Sorted Array](#Leetcode[26]-Remove_Duplicates_from_Sorted_Array)
- [Leetcode[27]-Remove Element](#Leetcode[27]-Remove_Element)
- [Leetcode[33]-Search in Rotated Sorted Array](#Leetcode[33]-Search_in_Rotated_Sorted_Array)
- [Leetcode[35]-Search Insert Position](#Leetcode[35]-Search_Insert_Position)
- [Leetcode[36]-Valid Sudoku](#Leetcode[36]-Valid_Sudoku)
- [Leetcode[53]-Maximum Subarray](#Leetcode[53]-Maximum_Subarray)
- [Leetcode[62]-Unique Paths](#Leetcode[62]-Unique_Paths)
- [Leetcode[63]-Unique Paths II](#Leetcode[63]-Unique_Paths_II)
- [Leetcode[66]-Plus One](#Leetcode[66]-Plus_One)
- [Leetcode[70]-Climbing Stairs](#Leetcode[70]-Climbing_Stairs)
- [Leetcode[74]-Search a 2D Matrix](#Leetcode[74]-Search_a_2D_Matrix)
- [Leetcode[81]-Search for a Range](#Leetcode[81]-Search_for_a_Range)
- [Leetcode[82]-Remove Duplicates from Sorted List II](#Leetcode[82]-Remove_Duplicates_from_Sorted_List_II)
- [Leetcode[83]-Remove Duplicates from Sorted List](#Leetcode[83]-Remove_Duplicates_from_Sorted_List)
- [Leetcode[86]-Partition List](#Leetcode[86]-Partition_List)
- [Leetcode[88]-Merge Sorted Array](#Leetcode[88]-Merge_Sorted_Array)
- [Leetcode[92]-Reverse Linked List II](#Leetcode[92]-Reverse_Linked_List_II)
- [Leetcode[94]-Binary Tree Inorder Traversal](#Leetcode[94]-Binary_Tree_Inorder_Traversal)
- [Leetcode[96]-Unique Binary Search Trees](#Leetcode[96]-Unique_Binary_Search_Trees)
- [Leetcode[98]-Validate Binary Search Tree](#Leetcode[98]-Validate_Binary_Search_Tree)
- [Leetcode[100]-Same Tree](#Leetcode[100]-Same_Tree)
- [Leetcode[101]-Symmetric Tree](#Leetcode[101]-Symmetric_Tree)
- [Leetcode[102]-Binary Tree Level Order Traversal](#Leetcode[102]-Binary_Tree_Level_Order_Traversal)
- [Leetcode[103]-Binary Tree Zigzag Level Order Traversal](#Leetcode[103]-Binary_Tree_Zigzag_Level_Order_Traversal)
- [Leetcode[104]-Maximum Depth of Binary Tree](#Leetcode[104]-Maximum_Depth_of_Binary_Tree)
- [Leetcode[107]-Binary Tree Level Order Traversal II](#Leetcode[107]-Binary_Tree_Level_Order_Traversal_II)
- [Leetcode[110]-Balanced Binary Tree](#Leetcode[110]-Balanced_Binary_Tree)
- [Leetcode[111]-Minimum Depth of Binary Tree](#Leetcode[111]-Minimum_Depth_of_Binary_Tree)
- [Leetcode[113]-Path Sum II](#Leetcode[113]-Path_Sum_II)
- [Leetcode[114]-Flatten Binary Tree to Linked List](#Leetcode[114]-Flatten_Binary_Tree_to_Linked_List)
- [Leetcode[118]-Pascal's Triangle](#Leetcode[118]-Pascal's_Triangle)
- [Leetcode[119]-Pascal's Triangle II](#Leetcode[119]-Pascal's_Triangle_II)
- [Leetcode[125]-Valid Palindrome](#Leetcode[125]-Valid_Palindrome)
- [Leetcode[128]-Longest Consecutive Sequence](#Leetcode[128]-Longest_Consecutive_Sequence)
- [Leetcode[136]-Single Number](#Leetcode[136]-Single_Number)
- [Leetcode[137]-Single Number II](#Leetcode[137]-Single_Number_II)
- [Leetcode[141]-Linked List Cycle](#Leetcode[141]-Linked_List_Cycle)
- [Leetcode[143]-Reorder List](#Leetcode[143]-Reorder_List)
- [Leetcode[144]-Binary Tree Preorder Traversal](#Leetcode[144]-Binary_Tree_Preorder_Traversal)
- [Leetcode[145]-Binary Tree Postorder Traversal](#Leetcode[145]-Binary_Tree_Postorder_Traversal)
- [Leetcode[147]-Insertion Sort List](#Leetcode[147]-Insertion_Sort_List)
- [Leetcode[148]-Sort List](#Leetcode[148]-Sort_List)
- [Leetcode[153]-Find Minimum in Rotated Sorted Array](#Leetcode[153]-Find_Minimum_in_Rotated_Sorted_Array)
- [Leetcode[154]-Find Minimum in Rotated Sorted Array II](#Leetcode[154]-Find_Minimum_in_Rotated_Sorted_Array_II)
- [LeetCode[155]-Min Stack](#LeetCode[155]-Min_Stack)
- [Leetcode[162]-Find Peak Element](#Leetcode[162]-Find_Peak_Element)
- [Leetcode[169]-Majority Element](#Leetcode[169]-Majority_Element)
- [Leetcode[173]-Binary Search Tree Iterator](#Leetcode[173]-Binary_Search_Tree_Iterator)
- [Leetcode[189]-Rotate Array](#Leetcode[189]-Rotate_Array)
- [Leetcode[191]-Number of Bits](#Leetcode[191]-Number_of_Bits)
- [Leetcode[198]-House Robber](#Leetcode[198]-House_Robber)
- [Leetcode[202]-Happy Number](#Leetcode[202]-Happy_Number)
- [Leetcode[203]-Remove Linked List Elements](#Leetcode[203]-Remove_Linked_List_Elements)
- [Leetcode[206]-Reverse Linked List](#Leetcode[206]-Reverse_Linked_List)
- [Leetcode[215]-Kth Largest Element in an Array](#Leetcode[215]-Kth_Largest_Element_in_an_Array)
- [Leetcode[217]-Contains Duplicate](#Leetcode[217]-Contains_Duplicate)
- [Leetcode[219]-Contains Duplicate II](#Leetcode[219]-Contains_Duplicate_II)
- [Leetcode[222]-Count Complete Tree Nodes](#Leetcode[222]-Count_Complete_Tree_Nodes)
- [Leetcode[226]-Invert Binary Tree](#Leetcode[226]-Invert_Binary_Tree)
- [Leetcode[231]-Power of Two](#Leetcode[231]-Power_of_Two)
- [Leetcode[237]-Delete Node in a Linked List](#Leetcode[237]-Delete_Node_in_a_Linked_List)
- [Leetcode[242]-Valid Anagram](#Leetcode[242]-Valid_Anagram)
- [Leetcode[258]-Add Digits](#Leetcode[258]-Add_Digits)
- [Leetcode[260]-Single Number III](#Leetcode[260]-Single_Number_III)
- [Leetcode[263]-Ugly Number++](#Leetcode[263]-Ugly_Number++)
- [Leetcode[283]-Move Zeroes](#Leetcode[283]-Move_Zeroes)
- [Leetcode[292]-Nim Game](#Leetcode[292]-Nim_Game)
- [Leetcode[300]-Longest Increasing Subsequence](#Leetcode[300]-Longest_Increasing_Subsequence)

下面是具体题解内容。

---

# Leetcode[1]-Two Sum

Link:https://leetcode.com/problems/two-sum/  

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2

------
分析：
方法一：使用两个for循环，依次比较，不过这个方法在leetcode上超时了

c++

```
vector<int> twoSum(vector<int>& nums, int target) {
    vector<int> index(2);
    int n = nums.size();
    for(int i = 0; i < n ; i++) {
        index[0] = i+1;
        for(int j = i+1 ; j < n ; j++) {
            if(nums[i] + nums[j] == target){
                index[1] = j+1;
                return index;
            }
        }
    }
    return index;
}
```

法二：使用map存储所有的数组值和下标值，然后循环在map中找看能否找到target-nums[i]的map，如果找到了就终止循环，没找到继续找；最后返回index数组；

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> index(2);
        int n = nums.size();
        map<int,int> mapv;
        for(int i = 0; i < n ; i++) {
            mapv[nums[i]] = i;
        }
        map<int,int>::iterator it;
        for(int i = 0; i < n; i++) {
            it = mapv.find(target - nums[i]);
            if(it != mapv.end() && i!=it->second){
                index[0]=min(i+1, it->second + 1);
                index[1]=max(i+1, it->second + 1);
                break;
            }
        }
        return index;
    }
};
```

---

# Leetcode[4]-Median of Two Sorted Arrays

Link: https://leetcode.com/problems/median-of-two-sorted-arrays/

There are two sorted arrays nums1 and nums2 of size m and n respectively. Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).


----

思路：先将两个数组合并，然后排序，最后找中位数

中位数：若有n个数，n为奇数，则选择第（n+1）/2个为中位数，若n为偶数，则中位数是（n/2以及n/2+1）的平均数；

合并两个vector：merge(nums1.begin(),nums1.end(),nums2.begin(),nums2.end(),nums.begin());


Code(c++):

```
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(),n = nums2.size();
        vector<int> nums(m+n);
    
        merge(nums1.begin(),nums1.end(),nums2.begin(),nums2.end(),nums.begin());
    
        sort(nums.begin(),nums.end());
    
        int mid = (m+n)/2;
        if((m+n)%2==0) {
            return double(nums[mid]+nums[mid - 1])/2;
        }else
            return nums[mid];
    }
};
```



---

# Leetcode[7]-Reverse Integer

Link: https://leetcode.com/problems/reverse-integer/

Reverse digits of an integer.

	Example1: x = 123, return 321
	Example2: x = -123, return -321

-----


取一个数的最后一位，用`x % 10`,取一个数的前n-1位（共n位），用`x/10`，每一次取的时候，都将上一次取的数乘以10，然后再加上末尾的数即可，代码如下：

Code(C++)

```
class Solution {
public:
    int reverse(int x) {
        long result = 0;
        while(x != 0)
        {
            result = result*10 + x % 10;
            x /= 10;
        }
        return (result > INT_MAX || result < INT_MIN)? 0 : result;
    }
    
};

```

---

# Leetcode[9]-Palindrome Number

Link: https://leetcode.com/problems/palindrome-number/

Determine whether an integer is a palindrome. Do this without extra space.

Some hints:
Could negative integers be palindromes? (ie, -1)

If you are thinking of converting the integer to string, note the restriction of using extra space.

You could also try reversing an integer. However, if you have solved the problem "Reverse Integer", you know that the reversed integer might overflow. How would you handle such case?

There is a more generic way of solving this problem.

-------------

思路：求出它的反转数，然后比较两个数是否恒等于即可。

C++代码：


```
class Solution {
public:
    bool isPalindrome(int x) {
        
        if(x < 0) return false;
        int y = x, result = 0;
        while( y > 0 ){
            result = result * 10 + y%10;
            y /= 10;
        }
        return result == x;
    }
};

```

法二：将数字转换成字符串，遍历一遍字符串，看首位是否相等即可

```
    bool isPalindrome(int x) {
        
        if(x < 0) return false;
        if(x>=0 && x<=9) return true;
        string str;
        stringstream ss;
        ss<<x;
        ss>>str;
        int len = str.length();
        for(int i = 0; i < (len/2); i++ ){
            if(str[i] != str[len-1-i])return false;
        }
        return true;
        
    }
```

---

# Leetcode[12]-Integer to Roman+++

Link: https://leetcode.com/problems/integer-to-roman/


Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.


---

分析：


C++:

```
class Solution {
public:
    string intToRoman(int num) {
        int s[7]={0};
        int Roman[7]={1000,500,100,50,10,5,1};
        char ch[7]={'M','D','C','L','X','V','I'};
        int index=0;
        string result;
        while(index<7) {
            s[index]=num/Roman[index];
            num%=Roman[index++];
        }
        for(int i=0;i<7;i++){
            if (s[i]==1&&i+1<7&&s[i+1]==4)
                continue;
            if(s[i]==4){
                if (s[i-1]==1)
                    result=result+ch[i]+ch[i-2];
                else
                    result=result+ch[i]+ch[i-1];
            }else{
                for(int j=0;j<s[i];j++)
                    result+=ch[i];
            }
        }
        return result;
    }
};
```

---

# Leetcode[13]-Roman to Integer+++

Link: https://leetcode.com/problems/roman-to-integer/


Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.


---

分析：

从前往后扫描，用一个临时变量记录分段数字。

如果当前处理的字符对应的值和上一个字符一样，那么临时变量加上这个字符。比如III = 3
如果当前比前一个大，说明这一段的值应该是当前这个值减去前面记录下的临时变量中的值。比如IIV = 5 – 2
如果当前比前一个小，那么就可以先将临时变量的值加到结果中，然后开始下一段记录。比如VI = 5 + 1


C++:

```
class Solution {
public:
    int romanToInt(string s) {
        int num[256] = { 0 };
        num['I'] = 1; num['V'] = 5; num['X'] = 10; num['L']=50;
        num['C'] = 100; num['D'] = 500; num['M'] = 1000;
        int result = 0;
        int i = 0;
        while (i < s.size()){
            if (num[s[i]] < num[s[i+1]])
                result -= num[s[i]];
            else
                result += num[s[i]];
            i++;
        }
        return result;
    }
};
```






Python：

```
class Solution(object):
    def romanToInt(self, s):
        """
        :type s: str
        :rtype: int
        """
        Num = {"I":1, "V":5, "X":10, "L":50, "C":100, "D":500, "M":1000}
        L = len(s)
        sum = 0
        i = 0
    
        while(i < L - 1):
    
            if(Num.get(s[i]) < Num.get(s[i + 1])):
                sum -= Num.get(s[i])
            else:
                sum += Num.get(s[i])
            i += 1
        sum += Num.get(s[i])
        return sum
```

---

# Leetcode[15]-3Sum

Link:https://leetcode.com/problems/3sum/

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:
Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)
The solution set must not contain duplicate triplets.
    For example, given array S = {-1 0 1 2 -1 -4},

    A solution set is:
    (-1, 0, 1)
    (-1, -1, 2)

----------

分析：根据题意，我们可以要找出三个数相加等于0的这样的一个集合，所以采用二维数组存储。

首先对数组进行从小到大排序，然后抽取一个变量出来，该变量从左往右递归遍历，递归的同时设置两个变量，让其中一个从第一个变量的右边开始，另一个从数组的末端开始，同步的向中间遍历，有点类似于快速排序的判断方式，

- 如果三个数相加小于零，则让第二个变量自加；
- 如果三个数相加大于零，则让第三个变量自减；
- 如果三个数相加等于零，则将三个数加入到数组中，然后让第二个变量和第三个变量同步增减，自增自减的过程中要判断是否有重复数字；

依次递归，直到第一个变量条件终止为止。


**Code(c++):**

```
class Solution {
public:
    vector<vector<int> > threeSum(vector<int> &nums) {
        vector<vector<int> >  result;

        sort(nums.begin(), nums.end());
    
        for(int i = 0; i < nums.size(); i++){
            if(i > 0 && nums[i] == nums[i-1]) continue;
            threeNumber(nums, result, i); 
        }
        return result;
    }
    //return vector<vector<int> > results 
    void threeNumber(vector<int> &nums, vector<vector<int> > &results, int curIndex) {
        int i = curIndex + 1;
        int j = nums.size()-1;
    
        while(i < j){
            if(nums[curIndex] + nums[i] + nums[j] < 0)   i++;
            else if(nums[curIndex] + nums[i] + nums[j] > 0)   j--;
            else{
                vector<int> v;
                v.push_back(nums[curIndex]);
                v.push_back(nums[i]);
                v.push_back(nums[j]);
                results.push_back(v);
                i++; j--;
                while(i < j && nums[i]==nums[i - 1]) i++;
                while(j > i && nums[j] == nums[j + 1]) j--;
            }
        }
    }
};
```

---

# Leetcode[18]-4Sum

Link: https://leetcode.com/problems/4sum/

Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.

Note:
Elements in a quadruplet (a,b,c,d) must be in non-descending order. (ie, a ≤ b ≤ c ≤ d)
The solution set must not contain duplicate quadruplets.

------
分析：和第15道题有点类似，仅仅是多了一层循环，注意该循环的条件就行.

C++:


```
class Solution {
public:
    vector<vector<int> > fourSum(vector<int>& nums, int target) {
        vector<vector<int> > result;    
        int n = nums.size();        
        sort(nums.begin(),nums.end());
        
        for(int i = 0; i < n; i++) {
            if(i>0 && nums[i] == nums[i-1]) continue;
            for(int j = i+1; j < n; j++){
                if(j > i+1 && nums[j]== nums[j-1]) continue;
                fourNumber(nums,result,i,j,target);
            }
        }
        return result;
    }
    
    void fourNumber(vector<int> & nums,vector<vector<int> > &results, int curIndex1,int curIndex2,int target){
        int i = curIndex2 + 1;
        int j = nums.size()-1;
        
        while(i<j) {
            if(nums[curIndex1] + nums[curIndex2] + nums[i] + nums[j] < target ) i++;
            else if(nums[curIndex1] + nums[curIndex2] + nums[i] + nums[j] > target ) j--;
            else {
                vector<int> vec;
                vec.push_back(nums[curIndex1]);
                vec.push_back(nums[curIndex2]);
                vec.push_back(nums[i]);
                vec.push_back(nums[j]);
                results.push_back(vec);
                i++,j--;
                while(i < j && nums[i] == nums[i-1])i++;
                while(j > i && nums[j] == nums[j+1])j--;
            }
        }
    }
};
```

---

# Leetcode[19]-Remove Nth Node From End of List

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

---

# Leetcode[20]-Valid Parentheses

Link: https://leetcode.com/problems/valid-parentheses/

Given a string containing just the characters `'(', ')'`, `'{', '}'`, `'[' and ']'`, determine if the input string is valid.

The brackets must close in the correct order, `"()"` and `"()[]{}"` are all valid but `"(]"` and `"([)]"` are not.

---------------

思路：借助vector容器，存放字符'(','{','['，从左到右的读取字符串的每一个字符，

- 如果字符为上面给定的三个，则加入到vector中；
- 如果不是，在确保vector容器有字符的情况下，则让其和vector的最后一个比较；

	- 如果是匹配的，则将vector容器的大小缩减为size()-1；
	- 如果不匹配，则返回false；

- 最后，如果vector元素全部匹配完了，则返回true，否则返回false；

代码如下（c++）：

```
class Solution {
public:
    bool isValid(string s) {
        vector<char> str;
        int len = s.length();
    
        for(int i=0; i<len; i++){
            if(isThose(s[i])) {
                str.push_back(s[i]);
                continue;
            }
            if(str.size()>0 && isTrue(str[str.size()-1],s[i])) {
                cout<<str.size()<<"f"<<isTrue(str[str.size()-1],s[i])<<endl;
                str.resize(str.size()-1);
            }else{
                return false;
            }
        }
        if(str.size() == 0)
            return true;
        else
            return false;
    }
    
    bool isThose(char &a){
        if(a == '{' || a == '(' || a == '[')return true;
        return false;
    }
    
    bool isTrue(char &a, char &b){
        if( a == '(' && b == ')' ) return true;
        else if( a == '[' && b == ']' ) return true;
        else if( a == '{' && b == '}' ) return true;
        
        return false;
    }
};
```

---

# Leetcode[21]-Merge Two Sorted Lists

Link: https://leetcode.com/problems/merge-two-sorted-lists/

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.


**思路**：要求合并两个排好序的链表。开始我们初始化头front和尾tail，然后从两个单链表的头部比较两个单链表，两链表同时不为空的条件下递归比较：

- 如果l1的值大于l2的值，就将尾部指向l1，并同步向右移动l1的头指针和tail指针
- 如果l1的值小于l2的值，就将尾部指向l2，并同步向右移动l2的头指针和tail指针

接着，如果l1不为空，则将尾指针的下一个指向l1，如果l2不为空，则将尾指针的下一个指向l2，然后将front右移一位，

最后返回front即可。

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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1;
        
        ListNode* front = new ListNode(-1);
        ListNode* tail = front;
        
        while( l1 && l2){
            if(l1->val < l2->val){
                tail->next = l1;
                l1 = l1->next;
                tail = tail->next;
            }else {
                tail->next = l2;
                l2 = l2->next;
                tail = tail->next;
            }
        }
        if(l1){
            tail->next = l1;
        }
        if(l2){
            tail->next = l2;
        }
        front = front->next;
        return front;
        
    }
};
```

---

# Leetcode[26]-Remove Duplicates from Sorted Array

Link: https://leetcode.com/problems/remove-duplicates-from-sorted-array/

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.

Do not allocate extra space for another array, you must do this in place with constant memory.

For example,
Given input array nums = [1,1,2],

Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

--------

**思路**：需要额外添加一个新的变量pos，初始值为0，用来记录非重复元素的位置。从数组的第二个元素开始遍历，如果和前面的元素相等，则直接跳到下一个；如果不等，则将该数组的值赋值给++pos位，接着继续遍历下一个；

**Code(c++):**

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        int pos = 0,i = 1;
        while(i < n){
            if(nums[i] == nums[i-1]){
                i++;
            }else{
                nums[++pos] = nums[i++];
            }
        }
        n = pos+1;
        nums.resize(n);
        return n;
    }
};
```

做个简单的修改：

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        int pos = 1,i = 1;//将pos从1开始
        while(i < n){
            if(nums[i] == nums[i-1]){
                i++;
            }else{
                nums[pos++] = nums[i++];//这里保持一致
            }
        }
        n = pos;
        nums.resize(n);
        return n;
    }
};
```

**Python代码**

```
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        if n == 0: 
            return 0
        pos = 1;i = 1;count=0
        while i < n:
            if nums[i] == nums[i-1]:
                i=i+1
                count=count+1
            else:
                nums[pos] = nums[i]
                pos = pos+1
                i = i + 1
        for i in range(count):
            nums.pop(-1)
        return len(nums)
```

---

# Leetcode[27]-Remove Element

Link: https://leetcode.com/problems/remove-element/

Given an array and a value, remove all instances of that value in place and return the new length.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

-------
思路：遍历数组，如果数组对应的数等于给定的值，数组最后一位和当前位互换，然后将数组长度减一；否则，就执行i++;最后重置数组长度。

Code(c++):
```
class Solution {
public:

    void swap(int *a, int *b){
        int temp = *a;
        *a = *b;
        *b = temp;
    }
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        if(n == 0) return 0;
        int i = 0;
        while( i < n){
            if(nums[i] == val) {
                swap(&nums[i],&nums[n-1]);
                n--;
            }else{
                i++;
            }
        }
        nums.resize(n);
        return n;
    }
};
```

---

# Leetcode[33]-Search in Rotated Sorted Array

Link: https://leetcode.com/problems/search-in-rotated-sorted-array/

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

--------

C++:

方法一：直接for循环求解

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        
        for(int i = 0; i<n; i++){
            if(nums[i] == target) return i;
        }
        return -1;
    }
};
```

方法二：二分搜索求解
分析：从中点处划分，左右两边一定有一部分是有序的，对于有序的分别做判断。

```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        
        int i = 0, j = nums.size()-1;
        
        while(i<=j){
            int mid = (i+j)/2;
            
            if(nums[mid] == target) 
                return mid;
            //if left is sorted
            else if(nums[i] <= nums[mid]){
                if(nums[i] <= target && target < nums[mid]) 
                    j = mid-1;
                else
                    i = mid + 1;
            }else {
                if(nums[mid] < target && target <= nums[j] ) 
                    i = mid +1;
                else 
                    j = mid-1;
            }
        }
        return -1;
    }
};
```

---

# Leetcode[35]-Search Insert Position

Link: https://leetcode.com/problems/search-insert-position/

Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.


	Here are few examples.
	[1,3,5,6], 5 → 2
	[1,3,5,6], 2 → 1
	[1,3,5,6], 7 → 4
	[1,3,5,6], 0 → 0


题目比较简单，直接给出答案吧
code(c++):

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        int i = 0;
        bool flag = false;
        while(i<n) {
            if( nums[i] >= target) {
                flag = true;
                break;
            }
            ++i;
        }
        return i;
    }
};
```

---

# Leetcode[36]-Valid Sudoku

Link:https://leetcode.com/problems/valid-sudoku/

Determine if a Sudoku is valid, according to: Sudoku Puzzles - The Rules.

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.
![这里写图片描述](http://img.blog.csdn.net/20150609210526658)

A partially filled sudoku which is valid.

Note:
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.


-------
分析：验证一个数独的合法性，从三个方向入手

- 每一行不能出现重复元素
- 每一列不能出现重复元素
- 每一个九方格不能出现重复元素

我采用的是map来保存元素的数值，然后判断map中是否有，有则返回false；参数之间的关系推导如下：
![这里写图片描述](http://img.blog.csdn.net/20150609213752137)
</a>
代码如下

Code(c++):
```
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        //row
        for(int  i = 0; i<9; i++) {
            map<int, int> mp;
            for (int j = 0; j<9; j++){
                if(mp.find(board[i][j])!=mp.end())
                    return false;
                if(board[i][j] == '.')continue;
                mp[board[i][j]] = j;
            }
        }
        
        //cow
        for(int  j = 0; j<9; j++) {
            map<int, int> mp;
            for (int i = 0; i<9; i++){
                if(mp.find(board[i][j])!=mp.end())
                    return false;
                if(board[i][j] == '.')continue;
                mp[board[i][j]] = i;
            }
        }
        
        //board
        for(int i = 0; i < 9; i++){
            map<int,int> mp;
            for(int j = (i/3)*3; j < 3 + (i/3)*3 ; j++){
                for(int k = (3*i)%9; k <= (3*(i+1)-1)%9; k++){
                    if(mp.find(board[j][k])!=mp.end())
                        return false;
                    if(board[j][k] == '.')continue;
                    mp[board[j][k]] = k;
                }
            }
        }
        
        return true;
        
    }
};
```

---

# Leetcode[53]-Maximum Subarray

Link: https://leetcode.com/problems/maximum-subarray/

Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array [−2,1,−3,4,−1,2,1,−5,4],
the contiguous subarray [4,−1,2,1] has the largest sum = 6.

More practice:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

--------

给定一个长度为n的序列，求其最大子序列和。
算法思路：使用动态规划求解，dp[i]表示在尾数在位置i上的最大子序列和，那么dp[i]可以表示为

- dp[i] = max(dp[i-1] + nums[i],nums[i])
- dp[0] = nums[0]

其中dp[0]表示初值。

**C++代码如下：**
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        
        int n = nums.size();
        vector<int> dp(n);
        dp[0] = nums[0];
        int answer = dp[0];
        
        for(int i=1; i<n; ++i){
            dp[i] = max(dp[i-1]+nums[i],nums[i]);
            answer = max(answer,dp[i]);
        }
        
        return answer;
        
    }
};
```

**Python代码如下：**

```
class Solution(object):
    def maxSubArray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        ans = nums[0]
        dp = []
        dp.append(ans)
        
        for i in range(1, n):
            dp.append(max(dp[i-1] + nums[i],nums[i]))
            ans = max(dp[i], ans)
        return ans
```

---

# Leetcode[62]-Unique Paths

Link: https://leetcode.com/problems/unique-paths/

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

![这里写图片描述](http://articles.leetcode.com/wp-content/uploads/2014/12/robot_maze.png)

Above is a 3 x 7 grid. How many possible unique paths are there?

Note: m and n will be at most 100.

---

题目：给定一个m*n的矩阵，让机器人从左上方走到右下方，只能往下和往右走，一共多少种走法。

思路：采用动态规划设计思想，到第0列和第0行的任何位置都只有1种走法，即初始化为d[0][\*] = 1,d[\*][0] = 1;当机器人走到第i行第j列的时候，它的走法总数是等于第i-1行第j列的总数加上第i行第j-1列的总数，即dp [ i ] [ j ]  = d[i-1][j] + dp[i][j-1].代码如下:


Code(c++): 

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int> > dp(m,vector<int> (n));
        for(int k = 0; k < m; k++) {
            dp[k][0] = 1;
        }
        for(int t = 0; t < n; t++) {
            dp[0][t] = 1;        
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

---

# Leetcode[63]-Unique Paths II

Link: https://leetcode.com/problems/unique-paths-ii/

Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

	[
	  [0,0,0],
	  [0,1,0],
	  [0,0,0]
	]

The total number of unique paths is 2.

题意：在上题的基础上增加了点限制条件，不过大体上还是差不多的，还是采用动态规划思想，找到初始值和递推关系，和上体相比较，只是初始化和到达dp[i][j]时要加点判断语句，

- 第0列，从上到下，如果碰到一个1，则设置该位置和后面的位置都不可到达，即dp[i to n][0]；
- 第0行，从左到右，如果碰到一个1，则设置该位置和右方的位置都不可到达，即dp[0][i to n];
- 其它位置，如果该位置不为1，则到达该位置的方式共有 dp[i][j] = dp[i-1][j] + dp[i][j-1],如果为1，则dp[i][j] = 0；

代码如下：

Code(c++):

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        
        int m = obstacleGrid.size(),n = obstacleGrid[0].size();
        vector<vector<int> > dp(m, vector<int>(n));
        
        bool flag = false;
        for(int i = 0; i < m; i++) {
            if(obstacleGrid[i][0] == 1) flag = true;
            
            if(flag == false )dp[i][0] = 1;
            else dp[i][0] = 0;
        }  
        
        flag = false;
        for(int j = 0; j < n; j++) {
            if(obstacleGrid[0][j] == 1) flag = true;
            if(flag == false )dp[0][j] = 1;
            else dp[0][j] = 0;
        }    
        
        for(int i = 1; i < m; i++) {
            for(int j = 1; j < n; j++){
                if(obstacleGrid[i][j] == 1) obstacleGrid[i][j] = 0; 
                else dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[m-1][n-1];
    }
};
```

---

# Leetcode[66]-Plus One

Link:https://leetcode.com/problems/plus-one/

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.


--------
题意：给定一个数组，表示的是非负数的各个位的数，现在将该数加一，求加一后得到的数组。

分析：由于加以后数组的长度可能发生变化，说以不能单纯的直接在数组后面加一。可以先将数组翻转，个位转到前面来，然后从前往后依次加一，最后判断如果在最后一位相加超过十了，就将数组长度加一，代码如下：

Code(c++):
```
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n = digits.size();
        reverse(digits.begin(),digits.end());
        int temp = 0,flag = false;
        for(int i = 0; i<n ; i++) {
            if(i==0) temp =1;
            int sum = digits[i] + temp;
            digits[i] = sum %10;
            temp = sum/10;
            if(i == n-1 && sum >= 10)
                flag = true;
        }
        if(flag){
            digits.resize(n+1);
            digits[n] = temp;
        }
        reverse(digits.begin(),digits.end());
        return digits;
    }
};
```

---

# Leetcode[70]-Climbing Stairs

Link: https://leetcode.com/problems/climbing-stairs/

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

------

题意：给你一个n阶的台阶，你一次最多只能上2个台阶，请问一共有多少个走法？

分析：典型的斐波那契数列题目，可以使用递归求解，也可以使用DP方法求解；

i表示阶梯数，f(i)表示有多少种走法；

- 当i = 1时，f(1) = 1,
- 当i = 2时，f(2) = 2;
- 当i > 3时，f(i) = f(i-1) + f(i-2)。

其实很容易理解，当阶梯数大于2时，它的走法可以是从n-1阶梯走一步，或是从n-2阶梯处一次走两步到达，即f(i) = f(i-1) + f(i-2)。

```
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n);
        for(int i = 0; i < n; i++){
            if(i < 2) dp[i] = i + 1;
            else dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n-1];
    }
};
```

---

# Leetcode[74]-Search a 2D Matrix

Link: https://leetcode.com/problems/search-a-2d-matrix/

Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,

Consider the following matrix:
```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
Given target = 3, return true.

-----

这道题之前做过：<a href="http://blog.csdn.net/dream_angel_z/article/details/46413705">查找特殊矩阵中的一个数</a>

算法思路：

起始从右上角开始查找，a[i][j]初试值为a[0][n-1]，循环下列
while( i < n && j >= 0) 
如果key < a[i][j],往左走，j–，
如果key > a[i][j],则往下走，执行i++
如果key == a[i][j],表示找到了

C++代码：

```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        
        int m = matrix.size(), n = matrix[0].size();
        
        int i=0,j = n-1;
        while( i < m && j >= 0){
            if( target < matrix[i][j] ) j--;
            else if( target > matrix[i][j] ) i++;
            else return true;
        }
        return false;
    }
};
```

---

# Leetcode[81]-Search for a Range

Link: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/

Given a sorted array of integers, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].

-------------

思路：首先初始化一个2列的数组，值为-1，然后一次遍历数组，设置一个变量作为标识，记录出现target值的下标，并保存到数组中，如果标识值等于=了，就不增加它的值，保证数组第二个元素是最后一个出现target的下标。

C++:

```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res(2);
        res[0]=-1,res[1]=-1;
        int n = nums.size();
        
        int temp = 0;
        for(int i = 0; i < n; i++){
            if(target == nums[i]){
                if(temp == 2)
                    res[temp-1] = i;
                else
                    res[temp++] = i;
            }
        }
        if(temp ==1) {
            res[temp] = res[0];
        }
        return res;
    }
};
```

---

# Leetcode[82]-Remove Duplicates from Sorted List II

Link: https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

For example,
Given `1->2->3->3->4->4->5`, return`1->2->5`.
Given `1->1->1->2->3`, return `2->3`.

-------

思路：跟上道题<a href="http://blog.csdn.net/dream_angel_z/article/details/46446067">Leetcode[83]-Remove Duplicates from Sorted List</a>算法有点区别，这道题需要设置一个标示符，如果某一趟比较的时候两个元素相等了，就设flag等于true，接着下一趟循环如果两个元素不相等，但此时的flag为true，就需要将两个元素前面的那个删掉。

最后，必定第二个元素为空而终止，此时还需要判断flag是否为true，如果为true，则末尾的元素还需要删掉；

代码如下：
Code(c++):

```c++
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
       ListNode * newList = new ListNode(-1);
        newList->next = head;
        ListNode *pre = newList;
    
        bool flag = false;
        while(pre->next && pre->next->next){
            if(pre->next->val == pre->next->next->val){
                pre->next = pre->next->next;
                flag = true;
                continue;
            }
            if(flag == true){
                pre->next = pre->next->next;
                flag = false;
                continue;
            }
            pre = pre->next;
        }
        //最后还需要判断是否上次判断时元素重复
        if(flag == true){
            pre->next = pre->next->next;
        }
        head = newList->next;
        return head;
    }
};
```

---

# Leetcode[83]-Remove Duplicates from Sorted List

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

---

# Leetcode[86]-Partition List

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

---

# Leetcode[88]-Merge Sorted Array

Link:https://leetcode.com/problems/merge-sorted-array/

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.

-------
分析：首先将nums1数组容量扩大到m+n,然后进行m+n-1次迭代，从后往前比较nums1和nums2的大小。

- 如果nums1的值大于nums2的值，就将nums1的值放到nums1的最后一个；
- 如果nums2的值大于nums1的值，就将nums2的值放到nums1的最后一个；
- ......
- 依次迭代
- ......

最后会必定有一个数组放完了，然后判断，哪个数组没放完，就接着放。

Code(c++):

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        nums1.resize(m+n);
        int j = m-1, k = n-1;
        for(int i = m+n-1; i >= 0; --i) {
            if(j >=0 && k >= 0){
                if(nums1[j] >= nums2[k]){
                    nums1[i] = nums1[j--];
                }else if(nums1[j] < nums2[k]){
                    nums1[i] = nums2[k--];
                }
            }else if(k>=0){
                nums1[i] = nums2[k--];
            }else if(j>=0){
                nums1[i] = nums1[j--];
            }
        }
    }
};
```

方法二：

```
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int s = m+n-1;
        nums1.resize(m+n);
        int i = m-1,j = n-1;
        while(i>=0 && j>=0){
            if (nums1[i] >= nums2[j]){
                nums1[s--] = nums1[i--];
            }else if(nums1[i] <nums2[j]){
                nums1[s--] = nums2[j--];
            }
        }  
        
        while(j>=0){
            nums1[s--] = nums2[j--];
        }
        while(i>=0){
            nums1[s--] = nums1[i--];
        }
    }
};
```

---

# Leetcode[92]-Reverse Linked List II

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

---

# Leetcode[94]-Binary Tree Inorder Traversal

Link: https://leetcode.com/problems/binary-tree-inorder-traversal/

Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree `{1,#,2,3}`,

```
   1
    \
     2
    /
   3
```

return` [1,3,2]`.

-------

递归遍历法C++：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> result;
    vector<int> inorderTraversal(TreeNode *root) {
        result.clear();
        inorder(root);
        return result;
    }
    void inorder(TreeNode* root){
        if(root == NULL) return;
        inorder(root->left);
        result.push_back(root->val);
        inorder(root->right);
    }
};
```


非递归算法1：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;  
        stack<TreeNode *> myStack;  
        if (root == NULL)  
            return result;  
        TreeNode *p = root;  
        while (p!=NULL || !myStack.empty()) {  
            while (p != NULL) {  
                myStack.push(p);  
                p = p->left;  
            }  
            if (!myStack.empty())  {  
                p = myStack.top();  
                myStack.pop();  
                result.push_back(p->val);  
                p = p->right;  
            }
        }
        return result;  
    }
};
```

非递归遍历方法二：(递归条件只需要判断栈是否为空)
在递归条件中，

- 首先取出栈顶节点，如果不为空的话，就把它的左节点进栈，然后再取栈顶元素，如果不为空，则再让它的左节点进栈，直到栈顶元素是空的节点为止；
- 然后让栈顶的空指针退栈
- 接着判断栈是否为空，如果不为空，则栈顶节点出栈，并将栈顶元素的值加入到数组中，然后把该节点的右节点入栈（右节点是否是空的，此处不做判断，内部的while循环会判断，因为内while循环后的栈顶节点必定是空指针）

最后，返回数组即可。

```
 /**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        if(root == NULL) return result; 
        
        stack<TreeNode *> stk;
        TreeNode* p = root;
        stk.push(root);
        while(!stk.empty()){
            while((p = stk.top()) && p){
                stk.push(p->left);
            }
            stk.pop();
            if(!stk.empty()){
                p = stk.top();
                result.push_back(p->val);
                stk.pop();
                p = p->right;
                stk.push(p);
            }
        }
        return result;
    }
};   
```

---

# Leetcode[96]-Unique Binary Search Trees

Link: https://leetcode.com/problems/unique-binary-search-trees/

Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,
Given n = 3, there are a total of 5 unique BST's.

	   1         3     3      2      1
	    \       /     /      / \      \
	     3     2     1      1   3      2
	    /     /       \                 \
	   2     1         2                 3



**思路：** 动态规划解题，dp[n]表示n个节点可以有dp[n]种不同的树，不管n为多少，先固定一个节点，剩余n-1个节点，分配给左右字数，然后把左子树个数乘以右子树的个数，

- 初始值dp[0] = 1,dp[1] = 1,
- dp[n] = dp[0] * dp[n-1] + dp[1] * dp[n-2] + ...+ dp[i] * dp[n-1-i] +... + dp[n-1] * dp[0]，也就是左边i个节点，右边n-1-i个节点。代码如下：

**Code(c++):**

```
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp;
        dp.resize(n+1);//set  the length of vector to n+1
        for(int i = 0; i <= n; i++) {
	        //dp[0] = 1 , dp[1] =1
            if(i<2){
                dp[i] = 1;
                continue;
            }
            //dp[n] = dp[0]*dp[n-1]+dp[1]*dp[n-2]+...+dp[n-1]*dp[0]
            for(int j = 1; j<=i; j++) {
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
};
```

---

# Leetcode[98]-Validate Binary Search Tree

Link: https://leetcode.com/problems/validate-binary-search-tree/

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
confused what "{1,#,2,3}" means? > read more on how binary tree is serialized on OJ.

-----

思路：递归方法，对于根结点

- 如果有左子树，比较根结点与左子树的最大值，如果小于等于则返回false；
- 如果有右子树，比较根结点与右子树的最小值 ，如果大于等于则返回false；
- 接着判断左子树和右子树是否也是合法的二叉搜索树；

Code(c++):

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if(root == NULL) return true;
        if(root->left && root->val <= leftMax(root->left)) return false;
        if(root->right && root->val >= rightMin(root->right)) return false;
        
        return isValidBST(root->left) && isValidBST(root->right);
    }
    //get the left tree max value
    int leftMax(TreeNode *root){
        if(root == NULL) return INT_MIN;
        int max = root->val;
    
        int lmax = leftMax(root->left);
        int rmax = leftMax(root->right);
    
        int max1 = lmax>rmax?lmax:rmax;
        return max>max1?max:max1;
    }
    //get the right tree minimum value
    int rightMin(TreeNode *root){
        if(root == NULL) return INT_MAX;
        int max = root->val;
    
        int lmin = rightMin(root->left);
        int rmin = rightMin(root->right);
    
        int max1 = lmin>rmin? rmin:lmin;
        return max>max1?max1:max;
    }
    
};
```

---

# Leetcode[100]-Same Tree

Link: https://leetcode.com/problems/same-tree/

Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.

-----

思路：递归法判断
假设两棵树根结点为p和q

- 如果p和q都为空，则返回true；否则，
- 如果p和q都非空，并且他们的值都相等，则判断其左右子树是否是相似树；否则
- 返回false；

代码如下(C++)：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q) return true;
        else if(p && q && (p->val == q->val)){
            return isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
        }else{
            return false;
        }
    }
};
```

---

# Leetcode[101]-Symmetric Tree

Link: https://leetcode.com/problems/symmetric-tree/

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
But the following is not:
    1
   / \
  2   2
   \   \
   3    3
```

Note:
Bonus points if you could solve it both recursively and iteratively.

-----

思路：从左右两边同时执行DFS，同步进栈出栈，如果发现不匹配，则返回false,最后如果匹配后，栈中还剩有节点，则也返回false，否则返回true。

Code（c++）：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(root==NULL) return true;
        
        TreeNode* p = root,*q = root;
        stack<TreeNode *> stk,stk2;//define two stack to save node
        stk.push(p);
        stk2.push(q);
        //if both of stk and stk2 is not empty 
        while(!stk.empty() && !stk2.empty() ){
	        //p go left,q go right
            while(p->left && q->right){
                p = p->left;
                stk.push(p);
                q = q->right;
                stk2.push(q);
            }
            //next line is very important to this method 
            if(p->left || q->right) return false;
            
            //both of stk and stk2 is not empty,pop from  stack and judge p's right node and q's left node
            if(!stk.empty() && !stk2.empty()){
                p = stk.top();
                q = stk2.top();
                stk.pop();
                stk2.pop();
                if(p->val != q->val) return false;
                
                if(p->right) stk.push(p->right);
                if(q->left) stk2.push(q->left);
            }
        }
        if(!stk.empty() || !stk2.empty()) return false;
        else return true;
    }
};
```

---

# Leetcode[102]-Binary Tree Level Order Traversal

Link: https://leetcode.com/problems/binary-tree-level-order-traversal/

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree {3,9,20,#,#,15,7},


	    3
	   / \
	  9  20
	    /  \
	   15   7

return its level order traversal as:


	[
	  [3],
	  [9,20],
	  [15,7]
	]


-----

**思路**： 使用层序遍历，一层一层的出队，并将节点值放入数组中；

代码C++：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int> > levelOrder(TreeNode* root) {
        vector<vector<int> > res;
        vector<int> nums;
        if(root == NULL) return res;
        queue<TreeNode *> que;
        TreeNode *p = root;
        que.push(p);
        while(!que.empty()){
            int queSize = que.size();
            nums.resize(0);
            for(int i = 0; i < queSize; i++){
                p = que.front();
                nums.push_back(p->val);
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
                que.pop();
            }
            res.push_back(nums);
        }
        return res;
    }
};
```


有问题，来一起讨论吧!

---

# Leetcode[103]-Binary Tree Zigzag Level Order Traversal

Link: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree {3,9,20,#,#,15,7},


	    3
	   / \
	  9  20
	    /  \
	   15   7

return its zigzag level order traversal as:

	[
	  [3],
	  [20,9],
	  [15,7]
	]


---

思路：使用层序遍历，借助队列，依次遍历每层，如果层数为偶数，则将该层的数字翻转，使用vector的reverse函数即可。

代码Code （C++）：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int> > res;
        if(!root) return res;
        vector<int> nums;
        queue<TreeNode *> que;
        TreeNode *p = root;
        que.push(p);       
        int level  = 0; // curent level
        while(!que.empty()){
            nums.resize(0);
            level += 1;
            int queSize = que.size();
            for(int i = 0; i < queSize; i++ ) {
                p = que.front();
                nums.push_back(p->val);
                que.pop();
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
            }
            if(level % 2 == 0) reverse(nums.begin(),nums.end());
            res.push_back(nums);
        }
        return  res;         
    }
};
```

---

# Leetcode[104]-Maximum Depth of Binary Tree

Link: https://leetcode.com/problems/maximum-depth-of-binary-tree/

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.


--------

求最大深度，用递归的方法

C++:

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root) {
        
        if(root==NULL) return 0;
        int ldepth = 0,rdepth = 0;
        if(root->left!=NULL){
            ldepth = maxDepth(root->left);
        }
        if(root->right!=NULL){
            rdepth = maxDepth(root->right);
        }
        return ldepth>rdepth?ldepth+1:rdepth+1;   
    }
};
```

---

# Leetcode[107]-Binary Tree Level Order Traversal II

Link: https://leetcode.com/problems/binary-tree-level-order-traversal-ii/

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree {3,9,20,#,#,15,7},

	    3
	   / \
	  9  20
	    /  \
	   15   7

return its bottom-up level order traversal as:

	[
	  [15,7],
	  [9,20],
	  [3]
	]


------
使用BFS层序遍历，然后得到的vector数组使用reverse反转即可。

Code(c++):

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int> > res;
        if(!root) return res;
        vector<int> nums;
        queue<TreeNode *> que;
        
        TreeNode *p = root;
        que.push(p);
        
        while(!que.empty()){ 
            int queSize = que.size();
            nums.resize(0);
            for(int i = 0; i < queSize; i++) {
                p = que.front();
                nums.push_back(p->val);
                if(p->left) que.push(p->left);
                if(p->right) que.push(p->right);
                que.pop();
            }
            res.push_back(nums);
        }
        reverse(res.begin(),res.end());
        return res;
        
    }
};
```

---

# Leetcode[110]-Balanced Binary Tree

Link: https://leetcode.com/problems/balanced-binary-tree/

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

-------

判断一棵树是否属于平衡二叉树

判断主节点的左右节点深度大小差，如果不在【-1,1】内，返回false，否则，继续判断其左右节点是否属于平衡二叉树；

只要有不满足的，就返回false

Code(C++):

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if(root == NULL) return true;
        
        int ldepth = getDepth(root->left);
        int rdepth = getDepth(root->right);
        
        if(ldepth - rdepth > 1 || ldepth - rdepth < -1) return false;
        
        return isBalanced(root->left) && isBalanced(root->right);
    }
    
    //get node's depth
    int getDepth(TreeNode *root){
        if(root==NULL) return 0;
    
        int ldepth=0,rdepth=0;
        ldepth = getDepth(root->left);
        rdepth = getDepth(root->right);
 
        return ldepth > rdepth ? ldepth+1:rdepth+1;
        
    }
};
```



---

# Leetcode[111]-Minimum Depth of Binary Tree

Link: https://leetcode.com/problems/minimum-depth-of-binary-tree/

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.



-----

C++:

递归法

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int minDepth(TreeNode* root) {
        
        if(root == NULL) return 0;
        if(!root->left && !root->right) return 1;
        
        int dleft=0,dright=0,dmin = 0;
        
        if(root->left) dleft = minDepth(root->left) + 1 ;
        if(root->right) dright = minDepth(root->right) + 1;
        
        if(!root->left && root->right) return dright;
        if(!root->right && root->left) return dleft;
        
        dmin = dleft>dright?dright:dleft; 
        return dmin;
        
    }
};
```



---

# Leetcode[113]-Path Sum II

Link: https://leetcode.com/problems/path-sum-ii/

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,


              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1

return


	[
	   [5,4,11,2],
	   [5,8,4,5]
	]


------------

思路：使用深度优先遍历，需要

- 一个map，保存节点是否出栈过；
- 一个一维数组，保存根结点到当前节点的路径；
- 一个二维数组，保存根结点到当前叶节点的和等于给定sum的路径集合；
- 一个栈，用来辅助深度优先遍历；

按照深度优先的遍历方式遍历，

- 进栈的时候元素同时添加到一维数组中，并将该节点的map值设置为0,
- 当节点左右节点都为空的时候，判断一维数组里面的数据和是否等于给定sum，如果等于就把它丢到二维数组中，同时执行下一步出栈；
- 出栈的时候也同时缩小一维数组的长度，并将该节点的map值设置为1，表示该节点不能再次进栈了；

Code（c++）：非递归算法：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int> > pathSum(TreeNode* root, int sum) {
        vector<vector<int> > res;
        if(root==NULL) return res;
        vector<int> nums;
        map<TreeNode *, int> visited;
        stack<TreeNode *> stk;
        
        TreeNode* p = root;
        nums.push_back(p->val);
        stk.push(p);
        visited[p] = 0;
        
        while(!stk.empty()){
            p = stk.top();
            while(p->left && visited[p] == 0){
	            //this is very important to break this while
                if(visited[p->left]==1) break;
                p = p->left;
                nums.push_back(p->val);
                stk.push(p);
                visited[p] = 0;
            }
            if(!stk.empty()){
                p = stk.top();
                if(!p->left && !p->right && sumVector(nums) == sum){
                    res.push_back(nums);
                }
                if( p->right && (visited.find(p->right) == visited.end() || visited[p->right] == 0)){
                    p = p->right;
                    stk.push(p);
                    nums.push_back(p->val);
                    visited[p] = 0;
                    continue;
                }
                visited[p] = 1;
                stk.pop();
                nums.resize(nums.size()-1);
            }
        }
        return res;
    }
    
    int sumVector(vector<int> & nums){
        int n = nums.size();
        if( n == 0 ) return 0;
        int sum = 0;
        for(int i = 0; i < n; i++) {
            sum += nums[i];
        }
        return sum;
    }
};

```


递归方法：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int> > pathSum(TreeNode* root, int sum) {
        vector<vector<int> > result;
        vector<int> nums;
        getPath(result,nums,sum,0,root);
        return result;

    }
    
    void getPath(vector<vector<int> > &res, vector<int> &nums, int target, int sum, TreeNode *root) {
        
        if(root==NULL) return;
        sum += root->val;
        nums.push_back(root->val);
        if(sum == target && !root->left && !root->right){
            res.push_back(nums);
        }
        
        getPath(res, nums, target, sum, root->left);
        getPath(res, nums, target, sum, root->right);
        
        nums.pop_back();
        sum -= root->val;
    }
};
```

---

# Leetcode[114]-Flatten Binary Tree to Linked List

Link: https://leetcode.com/problems/flatten-binary-tree-to-linked-list/

Given a binary tree, flatten it to a linked list in-place.

For example,
Given

        1
        / \
       2   5
      / \   \
     3   4   6

The flattened tree should look like:

	   1
	    \
	     2
	      \
	       3
	        \
	         4
	          \
	           5
	            \
	             6



----------

**思路: **  首先找到根节点左节点的最右子节点，然后把根节点的右子树移到左子树的最右端；接着把根结点的左子树移到右节点上，并将左子树置为空树，同时将根结点往右下移一个节点，依次递归即可。代码如下：

Code(c++):

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    
    void flatten(TreeNode* root) {
        if(!root) return;
        while(root){
            if(root->left && root->right){
                TreeNode *p = root->left;
                while(p->right) 
                    p =p->right;
                p->right = root->right;
            }
            
            
            if(root->left)
                root->right = root->left;
            root->left = NULL;
            root = root->right;
        }
    }
};
```

---

# Leetcode[118]-Pascal's Triangle

Link: https://leetcode.com/problems/pascals-triangle/

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,
Return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
---------
分析：

- 第j=0列全为1，第j==i列时，都为1
- 其它列
	- a[2][1] = a[1][0]+a[1][1]
	- a[3][1] = a[2][0]+a[2][1]
	- a[3][2] = a[2][1]+a[2][2]
	- ......
	- 推算得出
	- ......
	- a[i][j] = a[i-1][j-1]+a[i-1][j]

**代码（c++）：**

```
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector <vector<int> > vec(numRows);
        for(int i = 0; i < numRows; i++) {
            vec[i].resize(i+1);
            for(int j = 0; j <= i ; j++){
                if(j==i || j==0)
                    vec[i][j] = 1;
                else{
                    vec[i][j] = vec[i-1][j-1] + vec[i-1][j];
                }
            }
        }
        return vec;
    }
};
```

**代码（Python）：**

```
class Solution(object):
    def generate(self, numRows):
        """
        :type numRows: int
        :rtype: List[List[int]]
        """
        b=[]
        for i in range(numRows):
            temp = []
            for j in range(i+1):
                if j==0:
                    temp.append(1)
                else:
                    if j < i:
                        temp.append(b[i-1][j-1]+b[i-1][j])
                    elif j==i:
                        temp.append(1)
            b.append(temp)
        return b
```

---

# Leetcode[119]-Pascal's Triangle II

Link: https://leetcode.com/problems/pascals-triangle-ii/

Given an index k, return the kth row of the Pascal's triangle.

For example, given k = 3,
Return [1,3,3,1].


Note:
Could you optimize your algorithm to use only O(k) extra space?

-------

分析：通过递归设置vector的值，变量i表示当前行数，同时根据行数可以得到当前的vector元素个数。如果我们从前往后遍历，当i增加的时候，我们的num[j] = num[j] + num[j-1]就会出问题，因为num[j+1]=num[j+1]+num[j],而num[j]已经更新了。

所以这里采用的是从后往前遍历，num[j] = num[j] + num[j-1],这样num[j-1] = num[j-1] + num[j-2]，不会受到前面的num[j]的变化而变化。

```
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> nums(rowIndex+1);
        for(int i = 0; i <= rowIndex; i++){
            nums[i] =1;
            for(int j = i-1; j >= 1; j--){
                nums[j] = nums[j] + nums[j-1];
            }
        }
        return nums;
    }
};
```

**Python代码**

```
class Solution(object):
    def getRow(self, rowIndex):
        """
        :type rowIndex: int
        :rtype: List[int]
        """
        if rowIndex < 0: return None
        nums = []
        for i in range(rowIndex+1):
            a = []
            for j in range(i+1):
                if j==0:
                    a.append(1)
                else:
                    if j < i:
                        a.append(nums[i-1][j-1]+nums[i-1][j])
                    else:
                        a.append(1)
            nums.append(a)
        return nums[-1]
```

---

# Leetcode[125]-Valid Palindrome

Link: https://leetcode.com/problems/valid-palindrome/

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

```
For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.
```

Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.

For the purpose of this problem, we define empty string as valid palindrome.


-----

 思路：定义两个标示符，一个指向字符串前面，一个指向字符串末尾，如果前后位置的字符不是字母或是数字，则直接跳过该字符，如果是大写，则转换成小写再比较，如果碰到不匹配的直接返回false。


Code(C++):

```
class Solution {
public:

    bool isPalindrome(string s) {
        
        int length = s.length();
        
        if(length == 0){  
            return true;  
        }  
        int i = 0, j = length-1;
        
        while(i <= j){
            if(!isStr(s[i])) i++;
            else if(!isStr(s[j])) j--;
            else if(s[i++] != s[j--]) return false;
        }
        return true;
    }
    
    
    bool isStr(char &a){
        if(a >= '0' && a <= '9' ) {
            return true;
        }else if(a >= 'a' && a <= 'z' ) {
            a -= 32;
            return true;
        } else if(a >= 'A' && a <= 'Z' ) {
            return true;
        } 
        return false;
    }
   
};

```

---

# Leetcode[128]-Longest Consecutive Sequence

Link:https://leetcode.com/problems/longest-consecutive-sequence/

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,
Given [100, 4, 200, 1, 3, 2],
The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.

Your algorithm should run in O(n) complexity.

---

思路：跟最大连续子序列和的思路类似，采用DP思想。设置一个长度为n的数组dp[n]，dp[i]表示在下标为i的时候，当前最大连续序列的长度为dp[i]，最后找到dp[i]中最大的那个值即可。

C++:

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int n=nums.size();
        sort(nums.begin(),nums.end());
        vector<int> dp(n);
        dp[0]=1;
        for(int i=1; i<n; i++){
            if(nums[i]==(nums[i-1]+1))dp[i]=dp[i-1]+1;
            else if (nums[i] == nums[i-1]){
                dp[i] = dp[i-1];
            }else{
                dp[i]=1;
            }
        }
        int maxl=0;
        for(int j=0; j<n;j++){
            maxl = max(maxl,dp[j]);
        }
        return maxl;
    }
};
```

---

# Leetcode[129]-Sum Root to Leaf Numbers

Link: https://leetcode.com/problems/sum-root-to-leaf-numbers/

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

```
    1
   / \
  2   3
```

The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.

--------

这道题让我真的醉了，先说下思路吧。

先求出每个叶子节点的值，然后将这些值相加。求叶子节点的值可以通过深度优先遍历，一遍往下，一遍增加各个节点的值，如果是叶子节点就将数组值的和加入到一个只有叶子节点的数组中，最后求这个只有叶子节点的数组的值得和。从头到尾自己写的代码，可以AC了，还没有进行优化，先做到这里吧。

代码（C++）：

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
	//return sum of newleafValue
    int sumNumbers(TreeNode* root) {
        if(root == NULL) return 0;
        if(!root->left && !root->right) return root->val;
    
        vector<int> nums = saveLeafNums(root);
        return sumV(nums);
    }
    //save leftnode to vector
    vector<int> saveLeafNums(TreeNode * root){
    
        vector<int> nums,nodeValue;
        int eachSum = 0;
        stack<TreeNode *> stk;
        map<TreeNode *, int> visited;
    
        TreeNode *p = root;
    
        stk.push(p);
        visited[p] = 0;
        nodeValue.push_back(p->val);
    
        while(!stk.empty()){
            while(p->left && visited[p]==0 ){
	            //if this node is visited,break 
                if(visited[p->left]==1) break;
                p = p->left;
                stk.push(p);
                visited[p] = 0;
                tenMutilVector(nodeValue);//enlarge vector element's value
                nodeValue.push_back(p->val);
            }
            if(!stk.empty()) {
                p = stk.top();
                if(!p->left && !p->right){
                    nums.push_back(sumV(nodeValue));
                }
                if(p->right && (visited.find(p->right)==visited.end() || visited[p->right]==0)){
                    p = p->right;
                    stk.push(p);
                    visited[p] = 0;
                    tenMutilVector(nodeValue);
                    nodeValue.push_back(p->val);
                    continue;
                }
                visited[p] = 1;
                stk.pop();
                tenDivideVector(nodeValue);
                nodeValue.resize(nodeValue.size()-1);
            }
        }
        return nums;
    }
    
    void tenMutilVector(vector<int> &nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++){
            nums[i] *= 10;
        }
    }
    
    void tenDivideVector(vector<int> &nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++){
            nums[i] /= 10;
        }
    }
    
    int sumV(vector<int> &nums){
        int n = nums.size();
        int sumValue = 0;
        for(int i = 0; i < n; i++){
            sumValue += nums[i];
        }
        return sumValue;
    }   
};
```


附加一个求深度的代码：

```C++
    int getDepth(TreeNode* root) {
        if(root == NULL) return 0;
    
        int ldepth = 0,rdepth = 0;
    
        ldepth = getDepth(root->left);
        rdepth = getDepth(root->right);
    
        return ldepth>rdepth?ldepth+1:rdepth+1;
    }
```


方法二：（递归法）

```
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        sum = 0;
        sumStep(root, 0);
        return sum;
    }
private:
    int sum;
    void sumStep(TreeNode *root, int num){
        if(!root) return;
        if(!root->left && !root->right) {
            sum = sum + num*10 + root->val;
        }else{
            sumStep(root->left, num*10 + root->val);
            sumStep(root->right, num*10 + root->val);
        }
        
    }
};
```

---

# Leetcode[136]-Single Number

Lind:https://leetcode.com/problems/single-number/

Given an array of integers, every element appears twice except for one. Find that single one.

Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

-----

分析：首先对数组进行排序，然后从开始进行两两比较，如果两个相等，则以步长为2往后比较，如果不相等，则返回第一个值。代码如下：

Code(c++)

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        
        sort(nums.begin(),nums.end());
        if(n==1) return nums[0];
        int i = 0;
        while(i<n) {
            if(nums[i] == nums[i+1])
                i = i+2;
            else
                return nums[i];
        }
        return 0;
    }
};
```

---

# Leetcode[137]-Single Number II

Link:https://leetcode.com/problems/single-number-ii/

Given an array of integers, every element appears three times except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

---

思路：先排序，然后一对一对的进行比较.

C++

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int n = nums.size();
        if(n==1) return nums[0];
        sort(nums.begin(),nums.end());
        int i = 0;
        while(i<n-2){
            if(nums[i]!=nums[i+1]){
                return nums[i];
            }else{
                i = i + 3;
            }
        }
        return nums[n-1];
    }
};
```

---

# Leetcode[141]-Linked List Cycle

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

---

# Leetcode[143]-Reorder List

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

---

# Leetcode[144]-Binary Tree Preorder Traversal

Link: https://leetcode.com/problems/binary-tree-preorder-traversal/

Given a binary tree, return the preorder traversal of its nodes' values.

For example:
Given binary tree` {1,#,2,3}`,

	   1
	    \
	     2
	    /
	   3

return [1,2,3].



-----------


##  C++递归遍历：

```
/**C++
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> nums;
    vector<int> preorderTraversal(TreeNode* root) {
        if(root == NULL) return nums;
        preOrder(root);
        return nums;
    }
    void preOrder(TreeNode * root){
        if(root == NULL) return;
        nums.push_back(root->val);
        if(root->left)  preOrder(root->left);
        if(root->right) preOrder(root->right);
    }   
};
```

## C++递归遍历二:

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> ves;
        preorder(root, ves);
        return ves;
    }
    void preorder(TreeNode* root, vector<int> &nums){
        if(!root) return;
        nums.push_back(root->val);
        preorder(root->left, nums);
        preorder(root->right, nums);
    }   
};
```

## 非递归遍历C++：

```
vector<int> preorderTraversal(TreeNode* root) {
    vector<int > nums;
    stack<TreeNode* > stk;
    if(root == NULL) return nums;
    TreeNode * p = root;
    stk.push(p);   
    while(!stk.empty()){
        p = stk.top();
        nums.push_back(p->val);
        stk.pop();
        if(p->right) stk.push(p->right);
        if(p->left) stk.push(p->left);
    }
}
```

---

# Leetcode[145]-Binary Tree Postorder Traversal

Link: https://leetcode.com/problems/binary-tree-postorder-traversal/

Given a binary tree, return the postorder traversal of its nodes' values.

For example:
Given binary tree `{1,#,2,3}`,

	   1
	    \
	     2
	    /
	   3

return `[3,2,1]`.

Note: Recursive solution is trivial, could you do it iteratively?

-------


方法一：递归遍历

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:

    vector<int> nums;
    vector<int> postorderTraversal(TreeNode* root) {
        postorder(root);
        return nums;
    }
    
    void postorder(TreeNode * root){
        if(root==NULL) return;
        if(root->left) postorder(root->left);
        if(root->right) postorder(root->right);
        nums.push_back(root->val);
    }
};
```

---

# Leetcode[147]-Insertion Sort List

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

---

# Leetcode[148]-Sort List

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

---

# Leetcode[153]-Find Minimum in Rotated Sorted Array

Link: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/

Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.


-----

C++:

方法一：直接遍历，暴力求解

```
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int min = nums[0];
        for(int  i = 1; i < n; i++) {
            if(nums[i]<min){
                min = nums[i];
            }
        }
        return min;
    }
};
```

---

# Leetcode[154]-Find Minimum in Rotated Sorted Array II

Link：　https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/


	Follow up for "Find Minimum in Rotated Sorted Array":
	What if duplicates are allowed?

	Would this affect the run-time complexity? How and why?
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

The array may contain duplicates.

------

Ｃ++：

方法一：直接遍历求解

```
class Solution {
public:
    int findMin(vector<int>& nums) {
        
        int n = nums.size();
        int min = nums[0];
        for(int  i = 1; i < n; i++) {
            if(nums[i]<min){
                min = nums[i];
            }
        }
        return min;
    }
};
```


---

# LeetCode[155]-Min Stack

Link: https://leetcode.com/problems/min-stack/

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.

------
思路：需要两个栈，一个用来作为一般的栈，另一个用来存放进栈到某一位置的当前站内最小元素，代码如下：

Code（c++）:


```
class MinStack {
public:
    std::stack<int> stk;
    std::stack<int> min;
    void push(int x) {
        stk.push(x);
        if(min.empty() || (!min.empty() && x <= min.top())){
            min.push(x);
        }
    }

    void pop() {
        if(!stk.empty()){
            if(stk.top() == min.top())min.pop();
            stk.pop();
        }
    }

    int top() {
        if(!stk.empty())
            return stk.top();
    }

    int getMin() {
        if(!stk.empty()) return min.top();
    }
};
```

---

# Leetcode[162]-Find Peak Element

Link: https://leetcode.com/problems/find-peak-element/

A peak element is an element that is greater than its neighbors.

Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that num[-1] = num[n] = -∞.

For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.

click to show spoilers.


-----

分析：

- 如果只有一个元素，直接返回0；
- 如果元素个数>=2，判断首尾是否大于它的附近元素，大于则返回下标；
- 循环遍历，下标从>=1到 < n-1，判断nums[i]是否同时大于它的附近元素，如果大于则返回该下标；
 
否则，返回-1；


Code(c++)

```
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        if(n==1) return 0;
        if(nums[0] > nums[1]) return 0;
        if(nums[n-1] > nums[n-2]) return n-1;
        
        for(int i=1; i<n-1; i++){
            if(nums[i]>nums[i-1] && nums[i]>nums[i+1]){
                return i;
            }
        }
        return -1;
    }
};
```

---

# Leetcode[169]-Majority Element

Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Credits:
Special thanks to @ts for adding this problem and creating all test cases.

-----
思路一：将数组排好序，中间的那个数一定就是我们需要的majority element。时间复杂度O(nlogn)

思路二：Moore voting algorithm--每找出两个不同的element，就成对删除即count--，最终剩下的一定就是所求的。时间复杂度：O(n)

Code1(C++):

```
int majorityElement1(vector<int>& nums) {
    int n = nums.size();
    sort(nums.begin(),nums.end());
    return nums[n/2];
}
```

Code2(C++)

```
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();  
        int count = 0,number;
        for(int i=0;i < n; i++){
            if(count == 0) {
                number = nums[i];
                count++;
            } else {
                number == nums[i] ? count++: count--;
            }
        }
        return number;
    }
};
```

---

# Leetcode[173]-Binary Search Tree Iterator

Link: https://leetcode.com/problems/binary-search-tree-iterator/

Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

Note: next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

------

**思路：** 遍历一遍，然后从小到大的放到队列里去，然后判断队列是否非空，不非空则前面的就是最小的，出队即可！

C++:

```
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class BSTIterator {
public:
    queue<int> que;
    BSTIterator(TreeNode *root) {
        stack<TreeNode *> stk;
        map<TreeNode *, int> visited;
        TreeNode *p;
        if(root) { 
            stk.push(root);
            visited[root] = 0;
        }
        while(!stk.empty()){
            p = stk.top();
            while(p->left && visited[p] == 0){
                if(visited[p->left] == 1) break;
                p = p->left;
                stk.push(p);
                visited[p]=0;
            }
            visited[p]=1;
            que.push(p->val);
            stk.pop();
            
            if(p->right && (visited.find(p->right)==visited.end() || visited[p->right]==0)){
                stk.push(p->right);
                visited[p->right] = 0;
                continue;
            }
        }
        
        
        
	/* another way     
	stack<TreeNode*> stk;
        while(root || !stk.empty() ) {
            if(root){
                stk.push(root);
                root = root->left;
            }else {
                root = stk.top();
                stk.pop();
                que.push(root->val);
                root = root->right;
            }   
        } */ 
    }

/** @return whether we have a next smallest number */
    bool hasNext() {
        if(!que.empty()) return true;
        return false;
    }
/** @return the next smallest number */
    int next() {
        int val = que.front();
        que.pop();
        return val;
    }
};

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = BSTIterator(root);
 * while (i.hasNext()) cout << i.next();
 */
```

---

# Leetcode[189]-Rotate Array

Link: https://leetcode.com/problems/rotate-array/

Rotate an array of n elements to the right by k steps.

For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].

Note:
Try to come up as many solutions as you can, there are at least 3 different ways to solve this problem.

[show hint]

Related problem: Reverse Words in a String II

Credits:
Special thanks to @Freezen for adding this problem and creating all test cases.

-------
分析： (使用三次反转)利用

$$ba=(b^{r})^{r}(a^{r})^{r}=(a^{r}b^{r})^{r}$$

先分别反转a、b，最后再对所有元素进行一次反转。此算法读写内存各约2*n次。

结论：将数组按照k值分成左右两部分，右边k个，左边n-k个，然后先将左右两部分各自翻转，再将整个数组整体翻转，即可得到结果。

Code（c++）：

```
class Solution
{
	public:
		void swap(int * s1,int * s2)
		{
			int temp = *s1;
			*s1 = *s2;
			*s2 = temp;
		}

		void rotate(vector<int> & nums,int k)
		{
			int n = nums.size();
			int left = n - (k % n);
			for(int i = 0,j = left - 1;i < j;i++,j--)
				swap(&nums[i],&nums[j]);
			for(int i = left,j = n - 1;i < j;i++,j--)
				swap(&nums[i],&nums[j]);
			for(int i = 0,j = n - 1;i < j;i++,j--)
				swap(&nums[i],&nums[j]);
		}
};
```



---

# Leetcode[191]-Number of Bits

Link:https://leetcode.com/problems/number-of-1-bits/

Write a function that takes an unsigned integer and returns the number of ’1' bits it has (also known as the Hamming weight).

For example, the 32-bit integer ’11' has binary representation 00000000000000000000000000001011, so the function should return 3.

---

分析：**在十进制转换为二进制时，如果n%2=1，则在二进制中有1**.根据这条规则，可以循环判断n，每判断一次，n=n/2，代码如下：

C++:


```
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while(n>0){
            if(n%2==1){
                count++;
            }
            n=n/2; 
        }
        return count;
    }
};
```


Python:

```
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        count = 0
        while n>0:
            if n%2==1:
                count=count+1
            n=n/2
        return count
```

---

# Leetcode[198]-House Robber

Link: https://leetcode.com/problems/house-robber/

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

-------

题目意思：假如现在你是一个强盗，一个武功超群，足智多谋的江洋大盗，现在你要去一条街上抢劫，这条街全是贪官污吏，一共有N家，每家都有一定数量的金子，如果相邻的两家在同一个晚上都被打劫了，那么就会有一个类似触发器的东西自动调动兵马来街上抓你。

请问，在不触发这个警报器的前提下，你能抢劫到多少money？


**动态规划思想解题**

**分析：**，有N家贪官，假设是从左到右，第i家贪官家里的钱数为m[i]，i从0到N-1，根据题意可知，肯定不能在今晚打劫两个相邻的贪官，也就是假如打劫了第i家，就不能打劫第i-1家和i+1家。

设dp[i]表示我从第1家到达第i家能强盗的最大money数；

- 当你打劫第一家的时候，i = 0，可以得到的钱dp[i] = m[0]；
- 当你到达第二家的时候，i = 1，此时能得到的钱数为max(m[0],m[1]),因为不能同时打劫第一家和第二家；
- 当到达第i家的时候，i＞＝２，此时能够得到的钱数应该为max{dp[i-1],dp[i-2]+m[i]},即要么是到达上一家时的最大money数，要么是到达上上家时的最大money+这一家的money数，两者中较大的那个；

所以最后我们要得到的就是dp[N-1]，即到达最后一家时的最大money数。

-------

Code(c++):

```
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 0;
        vector<int> dp(n);
        for(int i = 0; i < n; i++) {
            if(i == 0) dp[i] = nums[i];
            else if(i == 1) dp[i] = max(nums[i],nums[i-1]);
            else{
                dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
            }
        }
        return  dp[n-1];
    }
};
```

---

# Leetcode[202]-Happy Number

Link:https://leetcode.com/problems/happy-number/

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.


Example: 19 is a happy number

$1^2 + 9^2 = 82$
$8^2 + 2^2 = 68$
$6^2 + 8^2 = 100$
$1^2 + 0^2 + 0^2 = 1$

Credits:
Special thanks to @mithmatt and @ts for adding this problem and creating all test cases.

------

**分析**：题目说的是对任意一个正整数，不断各个数位上数字的平方和，若最终收敛为1，则该数字为happy number，否则程序可能从某个数开始陷入循环。

这里我们使用一个哈希map表存储已经出现过的数字，如果下次还出现，则返回false。

```
class Solution {
public:
    bool isHappy(int n) {

        if(n==1) return true;
        map<int,int> nums;

        while(n>0){
            int number = 0;
            # 计算一个数各个位的平方和
            while(n){
                number += (n % 10) * (n % 10);
                n/=10;
            }

            if(number == 1)
                return true;
            else if(nums.find(number)!=nums.end())
                return false;
            n = number;
            nums[n] = 1;
        }
    }
};
```

另外还有一种简单的算法：

--------
```
class Solution {
public:
    bool isHappy(int n) {
        while(n>6){  
            int next = 0;  
            while(n){
                next+=(n%10)*(n%10); 
                n/=10;
            }  
            n = next;  
        }  
        return n==1;  
    }
};
```

---

# Leetcode[203]-Remove Linked List Elements

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

---

# Leetcode[206]-Reverse Linked List

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




---

# Leetcode[215]-Kth Largest Element in an Array

Link: https://leetcode.com/problems/kth-largest-element-in-an-array/

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given`[3,2,1,5,6,4] `and `k = 2`, return `5`.

Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.

Credits:
Special thanks to @mithmatt for adding this problem and creating all test cases.


------

法一：使用STL的sort排序O(NlogN)

```
int findKthLargest(vector<int>& nums, int k) {
    sort(nums.begin(),nums.end());
    return nums[nums.size()-k];
}
```

法二：自己写快速排序O（NlogN）

```
int findKthLargest(vector<int>& nums, int k){
   
	quickSort(nums, 0 ,nums.size());
	return nums[nums.size()-k];
   
}
void quickSort(vector<int> &nums, int left, int right){
    int i = left, j = right - 1;
    if(i < j){
        int po = nums[i];
        while(i < j){
            while(i < j && nums[j] >= po) j--;
            if(i < j) {
                nums[i++] = nums[j];
            }
            while(i < j && nums[i] <= po ) i++;
            if(i < j){
                nums[j--] = nums[i];
            }
        }
        nums[i] = po;
        quickSort(nums, left, i);
        quickSort(nums, i+1, right);
        
    }
}
```

法三：使用建堆法  时间复杂度O(klogN)

```
int findKthLargest(vector<int>& nums, int k){
    make_heap(nums.begin(), nums.end());
    for(auto i=0; i<k-1;i++){
        pop_heap(nums.begin(), nums.end());
        nums.pop_back();
    }
    return nums.front();
}
```

方法四：此方法是论坛看到的，O（N）

```
int findKthLargest(vector<int>& nums, int k){
     int i, m, n, pivot, head =0, tail = nums.size()-1, maxV;

    while(1){
        m = head, n= tail;
        pivot = nums[m++];
        while(m <= n) {
            if(nums[m] >= pivot) m++;
            else if(nums[n] < pivot) n--;
            else {
                swap(nums[m++], nums[n--]);
            }
        }
        if(m-head == k) 
            return pivot;
        else if(m-head < k) {
            k -= (m-head); 
            head = m;  
        }
        else {
            tail = m-1;
            head = head+1;
        }
    }
    
}
```

---

# Leetcode[217]-Contains Duplicate

Link：https://leetcode.com/problems/contains-duplicate/

Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.


----
思路：先将数组排序，然后从第二个开始遍历，如果和前一个值相等，则返回true，终止；此方法时间复杂度为O（nlogn），空间复杂度为O（1）

```
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        int n = nums.size();
        int flag = false;
        
        std::sort(nums.begin(), nums.end());
    
        int i = 1;
        while(i < n){
            if(nums[i] == nums[i-1])
                return true;
            i++;
        }
        return flag;
    }

};
```

**小记**：自己做的时候，开始使用两个for循环遍历，结果超时了，后来使用自己写的快速排序，也超时了。最后使用了std::sort(nums.begin(), nums.end())自带的sort，结果成功了！不知道是快速排序哪里出了问题。

<br><br>
拓展：快速排序算法

```
void quickSort(vector<int> &nums,int low,int high){
    int i=low,j=high;
    if(i < j){
        int po = nums[low];
        while(i < j){
            while(nums[i]<nums[j] && po < nums[j]) j--;
            if(i<j){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
            while(nums[i]<nums[j] && nums[i] < po) i++;
            if(i<j){
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                j--;
            }
        }
        quickSort(nums,low,j-1);
        quickSort(nums,j+1,high);
    }
}
```

**Python代码**

```
class Solution(object):
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        n = len(nums)
        if n == 0 or n ==1:
            return False
        nums.sort()
        
        for i in range(1,n):
            if nums[i] == nums[i-1]:
                return True
        return False
```

---

# Leetcode[219]-Contains Duplicate II

Link:https://leetcode.com/problems/contains-duplicate-ii/

Given an array of integers and an integer k, find out whether there there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.

-------
**分析**：用C++中的map记录num和下标value，key值为数值，value为值在nums数组中的下标。首先遍历数组，如果在map中存在
【mapv.find(number) != mapv.end()】并且当前位置和map中找到的位置差小于等于k【i-mapv[number] <= k】，就返回true，不然就将该值加入到map中。依次循环...

Code(c++)

```
class Solution {  
public:  
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n = nums.size();
        map<int, int> mapv;
        for (int i = 0; i < n; i++){
            int number = nums[i];
            if (mapv.find(number) != mapv.end() && i-mapv[number] <= k){
                return true;
            }else{
                mapv[number] = i;
            }
        }
        return false;
    }
};  

```

---

# Leetcode[222]-Count Complete Tree Nodes

Link: https://leetcode.com/problems/count-complete-tree-nodes/

Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.



------

思路：分别计算左右子树，然后返回左右字数节点个数加一

递归法：（超时了）

```
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == NULL) return 0;
        
        int lcount=0,rcount=0;
        if(root->left != NULL){
            lcount = countNodes(root->left);
        }
        if(root->right != NULL){
            rcount = countNodes(root->right);
        }
        return lcount+rcount+1;  
    }
};
```

方法二：
先计算左右的深度是否相等，相等则为满二叉树，满二叉树的节点个数为深度的平方减一,即depth^2-1；如果不相等，则递归以同样的方式计算左子树和右子树，并返回两者个数之和加一。

**Code(c++):**

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        if(root == NULL) return 0;
		
        int ldepth = getLeftDepth(root);
        int rdepth = getRightDepth(root);
        //return the square of leftdepth -1
        if(ldepth == rdepth) return (1 << ldepth) - 1;
    
        return countNodes(root->left)+countNodes(root->right)+1;
    }
    //compute the depth of left tree
    int getLeftDepth(TreeNode *root){
        int depth = 0;
        while(root){
            depth++;
            root = root->left;
        }
        return depth;
    }
    //compute the depth of right tree
    int getRightDepth(TreeNode *root){
        int depth = 0;
        while(root){
            depth++;
            root = root->right;
        }
        return depth;
    }
};
```

---

# Leetcode[226]-Invert Binary Tree

Link: https://leetcode.com/problems/invert-binary-tree/

Invert a binary tree.

	     4
	   /   \
	  2     7
	 / \   / \
	1   3 6   9

to

	     4
	   /   \
	  7     2
	 / \   / \
	9   6 3   1

Trivia:
This problem was inspired by this original tweet by Max Howell:
Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so fuck off.


----

C++递归法求解：

```

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(!root) return NULL;
        
        invertNode(root);
        return root;
    }
    
    void invertNode(TreeNode *root){
        if(root == NULL) return;
        if(!root->left && !root->right) return;
    
        TreeNode *tempNode=NULL;
        
        if(root->right) tempNode = root->right;
        if(root->left){
            root->right = root->left;
            root->left = tempNode;
        }else{
            root->left = tempNode;
            root->right = NULL;
        }
        invertNode(root->left);
        invertNode(root->right);
    
    }
};
```

---

# Leetcode[231]-Power of Two

Link:https://leetcode.com/problems/power-of-two/

Given an integer, write a function to determine if it is a power of two.

---

分析：

- 如果n小于0，返回false；
- 如果n等于1或者等于2，返回true；
- 当n大于2时，根据n大于2的条件进行递归，，如果n%2不为0则直接返回false，否则将n/2赋值给n
	- 如果循环结束后还没有返回false，则返回true。


C++:

```
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if(n<=0) return false;
        if (n==2||n==1) return true;
        while(n>2){
            if(n%2!=0) return false;
            n/=2;
        }
        return true;
    }
};
```

---

# Leetcode[237]-Delete Node in a Linked List

Link:https://leetcode.com/problems/delete-node-in-a-linked-list/

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.

Subscribe to see which companies asked this question


---
比较简单，直接给出答案

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
    void deleteNode(ListNode* node) {
        if(node == NULL) return;
        ListNode *tmp = node->next;
        node->val = tmp->val;
        node->next = tmp->next;
        delete tmp;
    }
};
```

---

# Leetcode[242]-Valid Anagram

Link:https://leetcode.com/problems/valid-anagram/

Given two strings s and t, write a function to determine if t is an anagram of s.

For example,
s = "anagram", t = "nagaram", return true.
s = "rat", t = "car", return false.


Note:
You may assume the string contains only lowercase alphabets.

---

思路：这道题，思路比较简单，将s中字符串的单词个数映射到26个子母中，然后遍历t，出现一个字母，就将该字母数减1，最后判断是否全为0即可。

Python代码比较简单，就给出Python代码吧：

Python：

```
class Solution(object):
    def isAnagram(self, s, t):
        """
        :type s: str
        :type t: str
        :rtype: bool
        """
        if len(s) != len(t):
            return False
        word = [0]*26
        for i in range(len(s)):
            index1 = ord(s[i])-97
            index2 = ord(t[i])-97
            word[index1] += 1
            word[index2] -= 1
        return not any(word)
```

附加C++:

```
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length()!=t.length())return false;
        int word[26];
        memset(word, 0, sizeof(word));
        for(int i=0;i<s.length();i++){
            int index1 = s[i]-'a';
            int index2 = t[i]-'a';
            word[index1]+=1;
            word[index2]-=1;
        }
        int c=0;
        while(c<26){
            if(word[c++]!=0)return false;
        }
        return true;
    }
};
```


学习心得：有关字符串的题目，可以考虑将其映射到字母表中。

---

# Leetcode[258]-Add Digits

Link: https://leetcode.com/problems/add-digits/

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

For example:

Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.

Follow up:
Could you do it without any loop/recursion in O(1) runtime?

---

思路：此题和202题有点类似，可以参考。主要是在各个位数的想加上，有点技巧。


C++

```
class Solution {
public:
    int addDigits(int num) {
        int n=num;
        while(n>=10){
            int i = 0;
            while(n>0){
                i += n%10;
                n = n/10;
            };
            n = i;
        }
        return n;
    }
};
```

---

# Leetcode[260]-Single Number III



Link: https://leetcode.com/problems/single-number-iii/

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

For example:

Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

Note:
The order of the result is not important. So in the above example, [5, 3] is also correct.
Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

---

思路：首先对数组进行排序，然后一对一对的进行比较，如果两个相等，则以步长为2往后移动，如果不相等，则将当前的值加入到返回变量中，然后以步长为1往后移动。

C++:

```
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<int> res;
        int n = nums.size();
        int i = 0;
        while(i<n-1){
            if(nums[i]==nums[i+1]){
                i+=2;
            }else{
                res.push_back(nums[i]);
                i+=1;
            }
        }
        if(nums[n-1]!=nums[n-2]){
            res.push_back(nums[n-1]);
        }
            
        return res;
    }
};
```

---

# Leetcode[263]-Ugly Number++

Link:https://leetcode.com/problems/ugly-number/

Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note that 1 is typically treated as an ugly number.

---

```
class Solution {
public:
    bool isUgly(int num) {
        if (num == 0) return false;
        if (num == 1) return true;

        int pfactor[] = { 2, 3, 5 };
        for (auto val : pfactor) {
            while (num % val == 0) { num /= val; }
        }

        return num == 1;
    }
};
```

---

# Leetcode[283]-Move Zeroes

Link:https://leetcode.com/problems/move-zeroes/

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].

Note:

- You must do this in-place without making a copy of the array.
- Minimize the total number of operations.

---

C++代码：

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n=nums.size();
        if (n==0 || n==1) return;
        int i=0,s = n-1;
        while(i<=s){
            if(nums[i]==0){
                for(int j = i; j<s; j++){
                    nums[j] = nums[j+1];
                }
                nums[s]=0;
                s = s-1;
            }else{
                i=i+1;
            }
        }
    }
};
```

---

# Leetcode[292]-Nim Game

Link:https://leetcode.com/problems/nim-game/


You are playing the following Nim Game with your friend: There is a heap of stones on the table, each time one of you take turns to remove 1 to 3 stones. The one who removes the last stone will be the winner. You will take the first turn to remove the stones.

Both of you are very clever and have optimal strategies for the game. Write a function to determine whether you can win the game given the number of stones in the heap.

For example, if there are 4 stones in the heap, then you will never win the game: no matter 1, 2, or 3 stones you remove, the last stone will always be removed by your friend.

---

分析：经过分析之后发现，有以下规律：

- 如果n小于4，返回true；
- 如果n%4==0，返回false；
- 否则，返回true。

C++:

```
class Solution {
public:
    bool canWinNim(int n) {
        if(n<=3) return true;
        if(n%4==0) return false;
        return true;
    }
};
```

Python：

```
class Solution(object):
    def canWinNim(self, n):
        """
        :type n: int
        :rtype: bool
        """
        if n<4:
            return True;
        if n%4==0:
            return False;
        return True;
```

---

# Leetcode[300]-Longest Increasing Subsequence

Link：https://leetcode.com/problems/longest-increasing-subsequence/


Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given `[10, 9, 2, 5, 3, 7, 101, 18]`,
The longest increasing subsequence is `[2, 3, 7, 101]`, therefore the length is `4`. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

Follow up: Could you improve it to O(n log n) time complexity?

---

这道题和最长连续子序列有些区别，它不限制【连续】这个条件，只要递增即可。

思路：定义一个长度为n的int类型一维数组dp[n]，dp[i]用来表示第i个位置上的最长递增子序列长度。dp[i]的计算过程如下：

- 初始化dp[0]=1；
- 当i>0时，设置一个变量max_dp，初始值为1,记录的是以当前位置结尾时的最长递增子序列长度。通过循环遍历前面的i-1个数，如果位置i的数大于前面位置j的数，就比较max_dp和dp[j]+1,将大的值赋值给max_dp，最后将max_dp赋值给dp[i]；
- 最后，遍历dp[n]，找出最大的值，即为最长递增子序列的长度。


C++代码：


```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        if(n==0) return 0;
        vector<int> dp(n);
        dp[0]=1;
        for(int i=1; i<n; i++){
            int j=i-1,max_dp=1;            
            while(j>=0){
                if(nums[j]<nums[i]){
                    max_dp=(max_dp>(dp[j]+1)?max_dp:(dp[j]+1));
                }
                --j;
            }
            dp[i]=max_dp;
        }
        int max=0;
        for(int k=0;k<n;k++){
            max=(max>dp[k]?max:dp[k]);
        }
        return max;
    }
};
```

---





