---
layout: post
date: 2014-12-25 10:24
title: "排序算法-交换排序"
categories: 算法与数据结构
tag: 
	- 数据结构
	- 排序
	- C++
	- 冒泡排序
	- 快速排序
comment: true
---

**注：代码均使用C++编写.**

再次回顾下各个排序算法的时间复杂度和空间复杂度的表格：

![](http://www.csuldw.com/assets/articleImg/2014-12-21-performances-of-sort-algs.png)

<!-- more-->

## 冒泡排序

冒泡排序---从前往后冒泡，临近的数字两两进行比较,按照从小到大或者从大到小的顺序进行交换，每轮得到一个最大值

```
//传入的n是数组长度
void bubbleSort(int nums[], int n){
    for(int i=n-1; i>=0; --i){
        for(int j=0; j<i; j++){
            if(nums[j]>nums[j+1]){
                swap(nums[j],nums[j+1]);   //交换两个元素
            }
        }
    }
}
```


## 快速排序

思想：首先任意选取一个数据（一般情况下都是选用数组的第一个数）作为关键数据，然后将所有比它小的数都放到它的前面，所有比它大的数都放到它的后面，此过程叫做一趟快速排序。快速排序不是一种稳定的排序算法，多个相同的值的相对位置也许会在算法结束时产生变动。

一趟快速排序算法如下：

- 首先，设置两个变量i、j，排序开始的时候：i=0，j=N-1；
- 以第一个数组元素作为关键数据，赋值给pirot，即pirot=nums[0]；
- 从j开始向前搜索，即由后开始向前搜索(j--)，找到第一个小于pirot的值nums[j]，将nums[j]和nums[i]互换；
- 从i开始向后搜索，即由前开始向后搜索(i++)，找到第一个大于pirot的nums[i]，将nums[i]和nums[j]互换；
- 重复第3、4步，直到i=j； (3,4步中，没找到符合条件的值，即3中nums[j]不小于pirot,4中nums[i]不大于pirot的时候改变j、i的值，使得j=j-1，i=i+1，直至找到为止。如果找到符合条件的值，进行交换时i， j指针位置不变。另外，i==j这一过程一定正好是i+或j-完成的时候，此时令循环结束即可）。

快速排序---注意：传入的left为数组起点下标，right为数组末点下标。

```
void quikSort(int nums[], int left, int right){
    int first = left;
    int last = right;
    int pirot = nums[first]; 
    if(first < last){
        while(first < last){
            while(first < last && nums[last] >= pirot) last--;
            nums[first] = nums[last];
            while(first < last && nums[first] <= pirot) first++;
            nums[last] = nums[first];
        }
        nums[first] = pirot;
        quikSort(nums, left, first-1);    //对左侧快排
        quikSort(nums, first+1, right);   //对右侧快排
    }
}
```


## 测试

下面简单的测试下快速排序：

```
#include<iostream>
using namespace std;
void bubbleSort(int nums[], int n);
void quikSort(int nums[], int left, int right);
void print_array(int nums[], int n);
int main()
{
    int nums[]={2, 5, 4, 3, 7, 6, 9, 21, 22, 121, 10, 8, 1};    //quikSort(nums,0,sizeof(nums)/sizeof(nums[0])-1);
    int n = sizeof(nums)/sizeof(nums[0]);
    //bubbleSort(nums, n);
    quikSort(nums, 0, n-1);
    print_array(nums, n);
    return 0;
}

//将遍历数组写在了一个函数里，方便调用
void print_array(int nums[], int n) {
    for(int i = 0; i<n; i++){
        cout<<nums[i]<<" ";
    }
    cout<<endl;
}
```

输出结果：
<pre><code class="markdown">1 2 3 4 5 6 7 8 9 10 21 22 121  
Process returned 0 (0x0)   execution time : 0.132 s
Press any key to continue.
</code></pre>


---

