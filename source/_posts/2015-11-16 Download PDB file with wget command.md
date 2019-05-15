---
layout: post
date: 2015-11-16 10:24
title: "Download PDB file with wget command"
categories: BioInfo
tag: 
	- BioInfo
	- PDB
comment: true
---

在Linux服务器下，使用`wget`命令下载PDB文件，即蛋白质文件。

- 输入文件格式：一个存有`protein chain`的单独文件，每行的格式为：`1A34A`
- 输出文件：多个蛋白质文件，买一行下载一个蛋白质，格式：1A34.pdb
<!--more-->
download.py文件

```
#!/usr/bin/python
# -*- coding: utf-8 -*-
import os
def downloadPDB(namefile,outpath):
    if not os.path.exists(outpath):
        os.mkdir(outpath)
    os.chdir(outpath)
    inputfile = open(namefile,'r')
    for eachline in inputfile:
        pdbname = eachline.lower().strip()[0:4]
        os.system("wget http://ftp.wwpdb.org/pub/pdb/data/structures/all/pdb/pdb" + pdbname + ".ent.gz")
        os.system("gzip -d pdb" + pdbname + '.ent.gz')
        os.system("mv pdb" + pdbname + ".ent " + pdbname.upper() + '.pdb')
    inputfile.close()
if __name__=="__main__":
    chainfile = os.sys.argv[1] 
    outpath = os.sys.argv[2]
    proteinList = downloadPDB(chainfile,outpath)
"""
test sample
python download.py /ifs/home/liudiwei/DNA_BP/_data/Exp_DBPI/train_set.txt /ifs/home/liudiwei/DNA_BP/_data/Exp_DBPI/pdb_trainset   
"""
```

命令解释：首先打开文件，然后逐行读取蛋白链，根据蛋白链的前四个字符，得到蛋白质的名字，然后使用`wget`命令下载`.ent.gz`文件，最后使用`gzip`解压文件即可。

命令行输入：

```
python download.py ../filepath  ../dirpath
```

其中`filepath`表示的是一个存有多行，每行表示一个蛋白链的单独文件。

Top10格式如下：


	1A12A
	1BCH1
	1BF6A
	1BYPA
	1C7JA
	1CHMA
	1CMNA
	1CUHA
	1CZYA
	1D7EA


最后的输出文件Top5：


	1A12.pdb
	1BCH.pdb
	1BF6.pdb
	1BYP.pdb
	1C7J.pdb

