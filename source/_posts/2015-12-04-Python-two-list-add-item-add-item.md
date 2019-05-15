---
layout: post
date: 2015-12-04 09:03
title: "Python笔记-均值列表"
categories: Python
tags:
	- Python
---

一个小小的实例，做个小笔记！

比如有三个列表，列表元素均为数值型，三个列表的长度都一样，现在我想要求这三个列表的均值，即求一个均值列表，对应元素为上述三个列表对应元素的均值。

<!--more-->


代码实现如下：

```
def meanMethod(one,two,three):
    comb = zip(one,two,three)
    return [float(i+j+k)/3 for i,j,k in comb]
```

第二行使用的是zip函数，先将三个列表合并起来，zip函数返回的是一个列表，但里面的元素是一个元组。

第三行是列表推导式，计算comb每个元组的均值。