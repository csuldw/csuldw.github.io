---
layout: post
date: 2014-11-19 10:24
title: "Linx学习笔记-tree命令"
categories: Linux
tag: 
	- Linux
	- Shell
comment: true
---


## 安装

有时候，我们想要知道一个目录下面的详细情况，那么有什么好的方法呢？

很幸运，Linux shell 有一个tree 命令，专门用来打印目录树。如果你没有安装，可以使用`yum`来安装，命令如下：

<!-- more-->


```shell
yum -y install tree
```
## 使用

此时如果想打印某个目录下的所有文件，可以使用`tree`命令：

<pre><code class="markdown">[liudiwei@master _code]$ tree
.
`-- preprocessing
    |-- compareTwoFile.py
    |-- download.py
    |-- extractChainFromSeq.py
    |-- extractSeqByChain.py
    |-- formatChain.py
    |-- generateSeqFromDSSP.py
    |-- getProteinFromChain.py
    |-- getProteinNameFromDir.py
    |-- pdbToDSSP.py
    `-- _README.txt
</code></pre>


此外，如果只想要显示目录的话，可以使用添加`-d`参数：

<pre><code class="markdown">[liudiwei@master DNA_BP]$ tree -d
.
|-- _code
|   `-- preprocessing
|-- _data
|   `-- Exp_DBPI
|       |-- DBPI_Datasets
|       |-- dssp_testset
|       |   `-- format
|       |-- dssp_trainset
|       |   `-- format
|       |-- pdb_testset
|       `-- pdb_trainset
|-- _feature
|   `-- feature_extraction
`-- paper_Graham

14 directories
</code></pre>

如果你不想看到全部的文件？可以加上“-P 通配符”的方法来只列出某种文件：

<pre><code class="markdown">[liudiwei@master DNA_BP]$ tree -P "*.py"
.
|-- _code
|   `-- preprocessing
|       |-- compareTwoFile.py
|       |-- download.py
|       |-- extractChainFromSeq.py
|       |-- extractSeqByChain.py
|       |-- formatChain.py
|       |-- generateSeqFromDSSP.py
|       |-- getProteinFromChain.py
|       |-- getProteinNameFromDir.py
|       `-- pdbToDSSP.py
|-- _data
|   `-- Exp_DBPI
|       |-- DBPI_Datasets
|       |-- dssp_testset
|       |   `-- format
|       |-- dssp_trainset
|       |   `-- format
|       |-- pdb_testset
|       `-- pdb_trainset
|-- _feature
|   `-- feature_extraction
`-- paper2015_Graham
</code></pre>

## 详细参数

`tree`常用参数：

<pre><code class="markdown">
-a 显示所有文件和目录。

-A 使用ASNI绘图字符显示树状图而非以ASCII字符组合。

-C 在文件和目录清单加上色彩，便于区分各种类型。

-d 显示目录名称而非内容。

-D 列出文件或目录的更改时间。

-f 在每个文件或目录之前，显示完整的相对路径名称。

-F 在执行文件，目录，Socket，符号连接，管道名称名称，各自加上”*“,”/“,”=“,”@“,”|“号。

-g 列出文件或目录的所属群组名称，没有对应的名称时，则显示群组识别码。

-i 不以阶梯状列出文件或目录名称。

-I <范本样式> 不显示符合范本样式的文件或目录名称。

-l 如遇到性质为符号连接的目录，直接列出该连接所指向的原始目录。

-n 不在文件和目录清单加上色彩。

-N 直接列出文件和目录名称，包括控制字符。

-p 列出权限标示。

-P <范本样式> 只显示符合范本样式的文件或目录名称。

-q 用”?“号取代控制字符，列出文件和目录名称。

-s 列出文件或目录大小。

-t 用文件和目录的更改时间排序。

-u 列出文件或目录的拥有者名称，没有对应的名称时，则显示用户识别码。

-x 将范围局限在现行的文件系统中，若指定目录下的某些子目录，其存放于另一个文件系统上，则将该子目录予以排除在寻找范围外。
</code></pre>

---

