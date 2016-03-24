---
layout: post
date: 2015-06-10 20:03
title: "Python模拟ls命令" 
tags:
	- Python
categories: Python
---

模拟控制台命令

写一个程序 lsrm 要求如下:

	模拟linux的命令ls部分功能
	当使用命令
	lsrm -ll
	显示目录下所有 py 结尾的文件
	增加难度 (1.使用递归 显示所有目录里的 py 结尾文件)

<!--more-->

---

首先定义一个outputFile函数，参数只设置一个infile，表示的是文件名或者目录名，然后进行判断，如果是文件，而且以py结尾，则输出；否则，如果是目录，则循环遍历每个每个文件。代码如下：


```Python
# -*- coding: utf-8 -*-
"""
Created on Wed Oct 28 19:25:20 2015

@author: liudiwei
"""

import os

def outputFile(infile):
    if os.path.isdir(infile):
        filelist = getFileList(infile)
        for eachfile in filelist:
            os.chdir(infile)
            outputFile(eachfile)
            os.chdir("..")
    else:
        if ".py" in infile:
            print infile


if __name__=='__main__':
    filepath = r"F:\CSU\Academic\analysis\experiment\code"; 
    filelist = getFileList(filepath)
    while True:
        command = raw_input('# ' )
        if command == 'lsrm -ll':    
            outputFile(filepath)
        elif command == "stop":
            break
```

运行结果如下图所示：

<center>
![output](/assets/images/20151029094653.png)
</center>


注意：在寻找子目录的文件时，需将工作目切换到子目录，档子目录遍历完毕后，再前换到上一层目录os.chdir("..").

---

