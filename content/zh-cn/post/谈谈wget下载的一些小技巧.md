---
title: 谈谈wget下载的一些小技巧
date: 2020-04-30 13:04:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/02/1621376742.jpg
description: wget是Linux系列操作系统上的一个基本上可以称作“标配”的小软件，可以用来下载互联网上的文件。它支持HTTP、HTTPS、FTP和FTPS协议，可以说是非常实用。这篇文章中我将会简单介绍一下wget使用的一些小技巧。
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - wget
  - 下载
---

### 安装

如果你的Linux系统上还没有安装wget，可以使用如下命令安装：

```
apt install wget -y
```

或

```
yum install wget -y
```

某位大神创造了wget的多线程版本mwget，但是是基于Python的（也就是说支持Windows）。我们可以在已经安装了pip的系统上使用如下命令安装：

```
pip install mwget
```

使用方法同wget，但是把`wget xxxxx`改成`mwget xxxxx`或者`python -m mwget xxxxx`。

### 使用技巧

基本的文件下载：

```
wget <URL>
```

如果文件比较大，有可能会连接丢失，可以使用断点续传：

```
wget -c <URL>
```

如果想要把一整个网站（的静态页面部分）扒下来，可以使用：

```
wget -r <URL>
```

这将会把从指定的URL开始所有超链接引用的资源都下载下来，但是如果网页中的链接用了绝对地址（http://xxx.xxx/xxx这样的)就无法正常本地访问，因此需要加上参数`-k`：

```
wget -r -k <URL>
```

如果我们只是想下载一个网页而不是整个网站，我们需要使用到`-p`和`-np`指令：

```
wget -p -k -np <URL>
```

在上面的指令中，如果下载的页面有问题，那么就把`-np`去掉。
