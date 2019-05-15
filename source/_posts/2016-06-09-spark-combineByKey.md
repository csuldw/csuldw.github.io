---
layout: post
date: 2016-06-09 10:24
title: "Spark算子之combineByKey"
categories: Spark
tag: 
	- Spark
	- 算子
	- combineByKey
	- Scala
comment: true
---

在做数据分析时，往往会碰到很多K-V结构，而处理K-V这种Pair型的数据结构是非常常见的事。譬如对Pair数据按照key进行分组、聚合，或是根据key对value进行fold运算等。本文讲解的就是spark的combineByKey算子。下面首先会对combineByKey的各个参数进行简单的介绍，然后通过一个实例来加深对它的理解。


<!-- more -->

## combineByKey介绍

Spark的combineByKey属于Key-Value型算子，主要做的是聚集操作，像这种transformation不会触发作业的提交，在一点与groupByKey和reduceByKey类似。combineByKey函数主要有三个参数，分别是:

* `combiner function` : 组合器函数，用于将`RDD[K,V]`中的V转换成一个新的值`C1`；
* `mergeValue function` ：合并值函数，将一个`C1`类型值和一个V类型值合并成一个`C2`类型，输入参数为`(C1,V)`，输出为新的`C2`;
* `mergeCombiners function` ：合并组合器函数，用于将两个C2类型值合并成一个`C3`类型，输入参数为`(C2,C2)`，输出为新的`C3`.

下面通过一个实例来理解。

## Example

首先来看看代码，如下：

```
outInfo.combineByKey(
  (v) => (1, v),
  (acc: (Int, String), v) => (acc._1 + 1, acc._2),
  (acc1: (Int, String), acc2: (Int, String)) => (acc1._1 + acc2._1, acc2._2)
).sortBy(_._2, false).map{
  case (key, (key1, value)) => Array(key, key1.toString, value).mkString("\t")
}.saveAsTextFile("/out")
```

在上述代码中，outInfo 其实是一个RDD，数据类型`(K: String, V: String)`，下面是测试数据的格式：

```
("hello", "world")
("hello", "ketty")
("hello", "Tom")
("Sam", "love")
("Sam", "sorry")
("Tom", "big")
("Tom", "shy")
```

现在，我的目的是按key值统计数据并对key去重，然后将每个key的最后一次出现的value作为value的第二个元素，即(key，count，value)，可以通过**combimeByKey**将上列数据转换成下列结果：

```
"hello"	3	"Tom"
"Sam"	2	"soryy"
"Tom"	2	"shy"
```

每行数据以`\t`分隔。

**详细解释：**

- 首先定义combiner function表达式`(v) => (1, v)`,可以将一个("hello", "world")转换成 ("hello", (1, "world"));
- 然后定义mergeValue function表达式 `(acc: (Int, String), v) => (acc._1 + 1, acc._2)`, 可以将(("hello", (1, "world"))、("hello", "ketty")转换成("hello", (2, "ketty")); 
- 接着定义mergeCombiners function表达式`(acc1: (Int, String), acc2: (Int, String)) => (acc1._1 + acc2._1, acc2._2)`可以将("hello", (2, "ketty"))、("hello", (1, "Tom"))转换成 ("hello", (3, "Tom")). 
- 最后按`count`进行排序，并以 `"hello"	3	"Tom"` 格式化输出，中间以"\t"分隔。


