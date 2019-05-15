---
title: Leetcode[189]-Rotate Array
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

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




</article>
