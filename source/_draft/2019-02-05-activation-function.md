---
layout: post
date: 2018-04-05 23:56
title: "激活函数"
categories: ML
tag: 
	- Machine Learning
	- deep learning
	- 激活函数
comment: true
---

本文主要介绍几种常见的激活函数。

<!--more-->


## Sigmoid

**1）Sigmoid激活函数优缺点**

1. 优点
	- 结果输出[0, 1]之间
	- 可靠的概率解释
	- 求导简单
2. 缺点
	- 导致梯度消失
	- Sigmoid输出不是0居中
	- exp()指数计算代价大

## tanh函数


**2）tanh(x)函数**

1. 优点
	- 数据压缩至[-1, 1]
	- 输出0居中
2. 缺点 
	- 梯度消失


## ReLU函数

**3）Relu激活函数**

表达式：max(0, x)

1. 优点
    - 不饱和（正区间）
    - 计算效率高
    - 实践中收敛速度比sigmoid/tanh更快
2. 缺点
    - 输出并非0居中
    - ReLu神经元会“die”(解决方法，使用leaky ReLU max(0.01x, x))



实践经验：

- 通常使用ReLU，但是要注意学习率
- 尝试Leaky ReLU/Maxout/ELU
- 可以尝试tanh激活函数，但是别抱太大期望
- 谨慎使用sigmoid