---
layout: post
date: 2016-07-30 22:12
title: "Inverted Index（倒排索引）"
categories: Spark
tag: 
	- InvertedIndex
	- 倒排索引
	- Spark
	- Scala
comment: true
---

传统的正排索引指的是doc->word的映射，然而在实际工作中，仅仅只有正排索引是远远不够的，比如我想知道某个word出现在那些doc当中，就需要遍历所有的doc，这在实时性要求比较严的系统中是不能接受的。因此，就出现了倒排索引（inverted index ），详细内容参见[Wikipedia-Inverted index](https://en.wikipedia.org/wiki/Inverted_index)。本文主要讲解的是如何使用Scala编写Spark程序来实现倒排索引。

<!-- more -->

## 原理

**目的：**根据某个word来查找包含该word的document。

现在，假设有一个输入文件[input.data](https://github.com/csuldw/MachineLearning/blob/master/spark-demo/invertedIndex/data/input.data)，里面包含5篇document，该文件的具体内容如下：

	doc1	Apache Spark Scala Hadoop Java C Python Do And Will KNN
	doc2	SVM Scala News Play Akka Yes GBDT
	doc3	LDA SVM RF GBDT Adaboost Kmeans KNN 
	doc4	QQ BAT I Great All LDA
	doc5	Apache Hadoop MapReduce Git SVN SVM

文件每行包含一篇doc的标示符（如doc1）以及该doc中包含的word，并且doc与word之间以"\t"分隔，而word与word之间以空格符分隔。

现在，我们需要建立的倒排索引如下（从结果数据中取出的top 10）：

<pre><code class="markdown">(Akka,doc2)
(Python,doc1)
(QQ,doc4)
(RF,doc3)
(Apache,doc1|doc5)
(Will,doc1)
(Java,doc1)
(MapReduce,doc5)
(SVM,doc2|doc3|doc5)
(Scala,doc1|doc2)
(Git,doc5)</pre></code>

上述结果为k-v结构的pair对，key值为word，value为文档表示符，并且doc与doc之间以"|"分隔，以示区分。下面使用Spark来建立倒排索引。


## 实现

根据上面的分析，大概知道了该实例的目的，下面使用Scala编写Spark程序实现倒排索引。

```
/**
  * Created by liudiwei on 2016-07-30.
  */
import org.apache.spark.SparkContext
import org.apache.spark.SparkConf
import org.apache.spark.SparkContext._
import org.apache.spark.SparkContext
import org.apache.spark.rdd.RDD
import org.apache.commons.configuration.{ PropertiesConfiguration => HierConf }
import org.apache.spark.broadcast.Broadcast

object InvertedIndex{
  def main(args : Array[String]){
    val conf = new SparkConf().setAppName("invertedIndex")
      .set("spark.serializer", "org.apache.spark.serializer.JavaSerializer")
      .set("spark.akka.frameSize","256")
      .set("spark.ui.port","4071")
    val sc = new org.apache.spark.SparkContext(conf)
    val cfg = new HierConf(args(0))
    val inputfile = cfg.getString("inputfile")
    val result = sc.textFile(inputfile)
      .map(x => x.split("\t"))
      .map(x => (x(0), x(1)))
      .map(x => x._2.split(" ").map(y => (y, x._1)))
      .flatMap(x => x)
      .reduceByKey( (x, y) => x + "|" + y)
    result.collect.foreach(println)
    sc.stop()
  }
}
```

从上面的代码可以看出，在spark中，只需几个map操作再加上一个reduceByKey聚合函数就可以建立倒排索引，非常简洁，相比其他语言有很大的优势。整个代码的核心只有21-26行这一系列操作，可以说是非常精简。

最后，完整的spark源码，请见Github：[https://github.com/csuldw/MachineLearning/tree/master/spark-demo/invertedIndex](https://github.com/csuldw/MachineLearning/tree/master/spark-demo/invertedIndex)。

## 参考文献

- https://en.wikipedia.org/wiki/Inverted_index
- https://www.elastic.co/guide/en/elasticsearch/guide/current/inverted-index.html
- https://www.quora.com/Information-Retrieval-What-is-inverted-index


<p style="height: 14px;line-height: 14px; border-left: solid 3px #e41c1e; padding-left: 10px; color: #666; font-size: 14px;">版权声明：本文为博主原创文章，转载请注明来源。</p>