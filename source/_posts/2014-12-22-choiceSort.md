---
layout: post
date: 2014-12-22 18:24
title: "排序算法-选择排序"
categories: 算法与数据结构
tag: 
	- 数据结构
	- 排序
	- C++
	- 直接选择排序
	- 堆排序
comment: true
---

**注：代码均使用C++编写.**

继续回顾下各个排序算法的时间复杂度和空间复杂度的表格：

![](http://www.csuldw.com/assets/articleImg/2014-12-21-performances-of-sort-algs.png)

<!-- more-->

## 直接选择排序

原理：每轮比较之后都会记录最大值的下标，最后将该位置的元素与当前最后一个元素交换。

```
//传入的n是数组长度
void choiceSort(int nums[], int n){
    for(int i = n-1; i >= 0; --i){
        int t = i;
        for(int j = 0; j < i; j++){
            if(nums[j] > nums[t]){
                t = j;
            }
        }
        swap(nums[i], nums[t]);
    }
}
```


## 堆排序（大根堆）

思想：需要建立大根堆。首先初始化大根堆，然后将堆顶元素和最后一个元素交换；接着使用剩下的n-1个元素重新建立大根堆，然后又将堆顶元素与第n-1个元素交换，依次循环，直到剩下最后一个元素为止。

对的存储，使用数组，需要注意：

- 如果数组下标从0开始，则第i个节点的左孩子是`2 * i +1`，右孩子是`2* i + 2`;
- 如果数组下标从1开始，则第i个节点的左孩子是`2 * i `，右孩子是`2* i +１`.


首先需要建立一个堆调整函数。

```
//7.堆排序（大根堆）[数组下标从0开始],m表示起始下标，n表示终止数字的下标
void HeapAdjust(int nums[], int j, int n){
    int value = nums[j];
    int i = 2 * j + 1;
    while(i <= n){
        //left孩子为2i+1,right孩子为2i+2
        if(i < n && nums[i] < nums[i+1]){//找到孩子中较大的那个下标
            ++i;
        }
        if(value > nums[i]){             //左右孩子中获胜者与父亲的比较
            break;
        }
        //将孩子结点上位，则以孩子结点的位置进行下一轮的筛选
        nums[j]= nums[i];
        j = i;
        i = 2 * i + 1;  //因为下标是零开始，所以左孩子这里是2*i+1
    }
    nums[j]= value; //插入最开始不和谐的元素
}
```

```
//堆排序
void HeapSort(int *nums, int n){
    int i;
    //初始化大根堆
    for(int i = n/2; i >= 0; i--){
        HeapAdjust(nums, i, n-1); //注意，下标从0开始，所以是n-1
    }
    for(i = n-1; i >= 0; i--){
        //cout<<nums[0]<<" "<<endl;
        swap(nums[0], nums[i]);      //交换堆顶和最后一个元素，即每次将剩余元素中的最大者放到最后面
        HeapAdjust(nums, 0, i-1);    //重新调整堆顶节点成为大顶堆
    }
}
```

## 测试

下面简单的测试下快速排序：

```
#include<iostream>
using namespace std;
void choiceSort(int nums[], int n);
void HeapSort(int *a,int len);
void print_array(int nums[], int n);
int main()
{
    int nums[]={2, 5, 4, 3, 7, 6, 9, 21, 22, 121, 10, 8, 1};    //quikSort(nums,0,sizeof(nums)/sizeof(nums[0])-1);
    int n = sizeof(nums)/sizeof(nums[0]);
    //choiceSort(nums, n);
    HeapSort(nums, 0, n-1);
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
Process returned 0 (0x0)   execution time : 0.083 s
Press any key to continue.
</code></pre>


---

