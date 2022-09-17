---
title: 从零开始的爬虫教程（1）——从urllib开始
date: 2020-01-24
description: 随着网络的迅速发展，万维网成为大量信息的载体，如何有效地提取并利用这些信息成为一个巨大的挑战。在这样的背景下，网络爬虫(Internet Bot)应运而生。在本课程中我将会较为系统地介绍简单Python爬虫的制作。我们从爬取著名硬件测评网站TechPowerUp的GPU数据库开始。
image: https://futrime.gitee.io/image-cdn/2020/01/3169515150.jpg
draft: false
author: Futrime
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Python
  - 爬虫
  - 网络
---


随着网络的迅速发展，万维网成为大量信息的载体，如何有效地提取并利用这些信息成为一个巨大的挑战。在这样的背景下，网络爬虫(Internet Bot)应运而生。在本课程中我将会较为系统地介绍简单Python爬虫的制作。我们从爬取著名硬件测评网站[TechPowerUp][1]的GPU数据库开始。

urllib是Python自带的网络标准库，地位相当于curl在Linux界。Request等其他网络请求库都是基于urllib的。如果想要系统地了解urllib，可以参考[官方文档][3]。

首先引入urllib.request库
    import urllib.request
注意这里引入的是urllib.request，在某些版本的urllib中被表示为urllib2，但是在最新的Python版本（3.8.1）中已经将urllib2重命名为urllib.request。另外，在某些Python版本中可以直接引入urllib但是其他一些版本不可以，因此兼容性考虑还是引入urllib.request。

我们只需要一条简短的语句就可以爬取TechPowerUp的首页。
    html = urllib.request.urlopen('https://www.techpowerup.com').read()
这将会返回一个bytes类型字符串，我们不妨将其转为str类型：
    html = html.decode()
然后我们就可以得到TechPowerUp首页内容了。

我们可以将其写到文件里：
    f = open('test.txt', 'w')
    f.write(html)
    f.close()
或者进行某些处理。

我提前收集了一个[TechPowerUp的GPU数据库2012-2020年所有NVIDIA显卡的详情页链接列表][4]。
我们采用[此代码][5]进行爬取并将所有网页存放在`./pages`目录中，效果如下所示：

> [root@localhost ~]# python3 1.py Connecting to
> http://www.techpowerup.com/gpu-specs/geforce-605-oem.c359
>
> Connecting to http://www.techpowerup.com/gpu-specs/nvidia-gf119.g93
>
> Connecting to http://www.techpowerup.com/gpu-specs/geforce-615.c2145

随后我们就可以在`./pages`文件夹里面看到爬取的网页了。

利用urllib制作爬虫的最最最基本的方法就是这样了，在下一篇博文中，我将会具体探讨某些优化方法和一些问题的解决方法。

参考链接：
1. https://docs.python.org/3/library/urllib.html
1. https://www.techpowerup.com


  [1]: https://www.techpowerup.com
  [3]: https://docs.python.org/3/library/urllib.html
  [4]: https://futrime.gitee.io/image-cdn/2020/01/2394686189.txt
  [5]: https://futrime.gitee.io/image-cdn/2020/01/1814181521.zip
