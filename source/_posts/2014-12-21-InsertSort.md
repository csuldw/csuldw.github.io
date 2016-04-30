---
layout: post
date: 2014-12-21 11:24
title: "排序算法-插入排序"
categories: 算法与数据结构
tag: 
	- C++
	- 数据结构
	- 排序
	- 希尔排序
	- 插入排序
comment: true
---

**注：代码均使用C++编写.**

首先介绍一个常用排序算法的时间复杂度和空间复杂度的表格：

![](/assets/articleImg/2014-12-21-performances-of-sort-algs.png)

<!--more-->

对于插入排序，本文简单介绍两种：简单插入排序和希尔排序。同时，会附上实现源码。

## 1.知识点小记

- 使用`sizeof(nums)/sizeof(nums[0])`获得数组的长度；

- 数组作为参数有两种方法，一种是以数组名本身，一种是以指针；

- 如果要给一个函数传入一个数组，一般都是传入两个参数，一个数组指针或数组名，另一个是数组大小；


## 2.简单插入排序


直接插入排序(Insertion Sort)的基本思想是：每次将一个待排序的记录，按其关键字大小插入到前面已经排好序的子序列中的适当位置，直到全部记录插入完成为止。

实现：从头到尾遍历数组，设置一个变量作为哨兵，记录当前元素；然后从当前位置依次往前寻找插入点，如果哨兵元素值要小，就将前面的元素往后移动一位，直到哨兵元素大于前面的元素为止。

设数组为a[0…n-1]。

- 初始时，a[0]自成1个有序区，无序区为a[1..n-1]。令i=1
- 将a[i]并入当前的有序区a[0…i-1]中形成a[0…i]的有序区间。
- i++，并重复第二步直到i==n-1。

简单插入排序方法实现：

```
//insert sort
void insertSort(int nums[], int n){
    for(int i=1;i<n;i++){
        int temp = nums[i];
        int j=i-1;
        while(nums[j]>temp && j>=0){
            nums[j+1] = nums[j];
            j--;
        }
        nums[j+1] = temp;
    }
}
```


下面是一个完整的例子：

```
#include <iostream>
using namespace std;
void insertSort(int nums[], int n)；
int main()
{
    int nums[] = {9,2,7,4,5};
    int n = sizeof(nums)/sizeof(nums[0]);
    insertSort(nums, n);
    for(int i =0; i< (sizeof(nums)/sizeof(nums[0])); i++){
        cout<<nums[i]<<" ";
    }
    return 0;
}

//insert sort
void insertSort(int nums[], int n){
    for(int i=1;i<n;i++){
        int temp = nums[i];
        int j=i-1;
        while(nums[j]>temp && j>=0){
            nums[j+1] = nums[j];
            j--;
        }
        nums[j+1] = temp;
    }
}
```

简单插入排序最坏和平均时间复杂度都为O($n^2$),空间复杂度为O(1)，最好的时间复杂度为O(n).属于稳定的排序算法。

---

## 3.希尔排序


希尔排序思路：先将整个待排元素序列分割成若干个子序列（由相隔某个“增量”的元素组成的）分别进行直接插入排序，然后依次缩减增量再进行排序，待整个序列中的元素基本有序（增量足够小）时，再对全体元素进行一次直接插入排序。因为直接插入排序在元素基本有序的情况下（接近最好情况），效率是很高的，因此希尔排序在时间效率上比前两种方法有较大提高。

希尔排序方法：

```
//希尔排序
void shellSort(int nums[], int n){
    int d = n>>1;
    while(d>=1){
        for(int i=0; i<n-d; i++){
            for(int j=i+d; j<n; j+=d){
                if(nums[j-d]>nums[j]){
                    int temp = nums[j-d];
                    nums[j-d] = nums[j];
                    nums[j] = temp;
                }
            }
        }
        d = d>>1;
        print_array(nums,n);
    }
}
```

下面是一个完整的例子：

```
#include <iostream>
using namespace std;
void shellSort(int nums[], int n);
void print_array(int nums[], int n);
int main()
{
    int nums[] = {9,2,7,4,5};
    int n = sizeof(nums)/sizeof(nums[0]);
    shellSort(nums, n);
    for(int i =0; i< (sizeof(nums)/sizeof(nums[0])); i++){
        cout<<nums[i]<<" ";
    }
    return 0;
}

//希尔排序
void shellSort(int nums[], int n){
    int d = n>>1;
    while(d>=1){
        for(int i=0; i<n-d; i++){
            for(int j=i+d; j<n; j+=d){
                if(nums[j-d]>nums[j]){
                    int temp = nums[j-d];
                    nums[j-d] = nums[j];
                    nums[j] = temp;
                }
            }
        }
        d = d>>1;
        print_array(nums,n);
    }
}

void print_array(int nums[], int n){
    for(int i = 0; i<n; i++){
        cout<<nums[i]<<" ";
    }
    cout<<endl;
}
```

输出结果：

![](/assets/articleImg/2014-12-21-insert-sort.png)

简单插入排序平均时间复杂度为O($n^{1.3}$),空间复杂度为O(1)。最坏情况下的时间复杂度为O($n^2$),最好的时间复杂度为O(n).

---

## 4.补充知识点


**将数组作为参数传递**

两种形式：

- 使用数组名本身,如上方的函数形式：

```
void insertSort(int nums[], int n)
```

- 用指针作为参数,这就简单了,只需将上面方法中的数组修改成指针形式:


```
void insertSort(int *nums, int n)
```

---

