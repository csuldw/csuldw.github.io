---
layout: post
date: 2015-10-21 16:45:44
tag: 
	- GitHub
title: "Windows下使用 git push 命令的无密码设置"
categories: GitHub
---

在使用git时，每次进行git push时都需要输入用户名和密码，简直让人抓狂呀。下面介绍一种方法，可以避免用户名和密码输入，节省大量时间。

<!-- more -->


## 1.添加环境变量

首先在系统变量中添加一个环境变量HOME，内容为

```
HOME%USERPROFILE%
```

<center>
![配置环境变量](http://ww4.sinaimg.cn/large/637f3c58gw1exbx3roqvcj20bo0cadgy.jpg)
</center>

## 2.新建配置文件

由于使用的是Windows，所以进入%HOME%目录（如我的:C:\Users\username），新建一个名为"_netrc"的文件，文件中内容格式如下：

```
machine github.com
login your-username
password your-password
```

接着，打开git bash后，输入git push 命令就无需再输入用户名和密码了。

爽歪歪啦~


---

