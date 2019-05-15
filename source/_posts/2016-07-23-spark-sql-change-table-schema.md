---
layout: post
date: 2016-07-22 23:24
title: "SparkSQL之更改表结构"
categories: Spark
tag: 
	- Spark
    - SparkSQL
	- Schema
	- 表结构
	- Scala
comment: true
---

本文篇幅较短，内容源于自己在使用SparkSQL时碰到的一个小问题，因为在之后的数据处理过程中多次使用，所以为了加深印象，在此单独成文，以便回顾。至于文章中使用的方法，或许不是最好的，如果你有更好的方法，还请分享一下。

<!-- more -->

## 场景

最近，在使用SparkSQL进行数据处理和数据分析时，碰到这样一种情况：需要自己去改变DataFrame中某个字段的类型。简而言之，就是要更改SparkSQL的表结构。因此，出于学习目的，在这里做了一个简单的Demo。下面来看看这个实例。

## Example

假设你现在也碰到了这个问题，那么前面的很多文件读取和数据转换操作，这里就不提及了，直奔主题吧。

	......
	......

	此处省略相关jar包的引入

首先使用sparkSQL的jsonFile加载HDFS上的一个文件（此步在此直接省略了），得到如下的表结构：

```
scala> dfs.printSchema
root
 |-- name: string (nullable = true)
 |-- desc: string (nullable = true)
 |-- click: double (nullable = true)
 |-- view: double(nullable = true)
```

**目的**：将`click`和`view`转成的类型转成`Long`。

操作如下:

首先需要定义一个函数，将表内的`Double`类型转为`Long`类型，函数如下：

```
val toLong = udf[Long, Double](_.toLong)
```

然后使用`withColumn`变换字段类型，代码如下：

```
val dfs2 = dfs.withColumn("click", toLong(dfs("click"))).withColumn("view", toLong(dfs("view")))
```

使用`printSchema`查看表结构：

```
scala> dfs2.printSchema
root
 |-- name : string (nullable = true)
 |-- desc : string (nullable = true)
 |-- click: long (nullable = true)
 |-- view: long (nullable = true)
```


OK，一个简单的表结构变换便完成了，又get了一个小技巧。