---
title: Linux下搭建Python环境
date: 2020-02-09 22:17:00
draft: false
author: Futrime
description: 常在Linux下玩，怎能没Python？Python和PyPI已经成为Linux下的一个新的风尚。现在许多热门的领域都逐渐被Python和PyPI所占领。Python凭借着它的极高的开发效率，成功拿下了一个又一个新的领域。而且，Python的存在打破了众多爱好者和传统意义上“服务器”的界限。可以说，在将来的3-4年内，将会是Python的时代。这篇文章我将会讲述一下如何在Linux下搭建Python和PyPI环境。
image: https://futrime.gitee.io/image-cdn/2020/02/530053381.jpg
categories:
  - 搞机记录
  - 经验杂谈
tags:
  - Python
  - Linux
  - Python环境
  - PyPI
  - pip
---

### 安装Python
首先我们需要安装Python。根据Python官方的通告，Python 2将会在2021年1月1日停止所有支持，Python 3将会是大势所趋。而现在很多的流行Python库已经开始逐步取缔Python 2，因此我将直接从Python 3的安装开始讲起。另外，就目前来说，很多来不及更新的库往往兼容到Python 3.6（或Python 3.7），而很多新的库最低支持到Python 3.6，因此建议大家直接安装Python 3.6（或者Python 3.7）。

首先我们要确认系统中是否已经安装了Python 3，输入以下命令查询：
'''
python3 --version
'''
假如返回了类似 `Python 3.6.x` 这样的字样就说明已经安装了Python 3.6，返回了类似 `Python 3.7.x`字样就说明已经安装了Python 3.7。如果返回的是 `Python 3.y.z`，其中y ≤ 5，或者根本没有返回，那么可以采取如下方法安装或者更新：

Debian、Ubuntu家族操作系统：
```
sudo apt install python3.6 -y
```
RedHat、Fedora、SUSE家族操作系统（root级别用户下）：
```
yum install python3.6 -y
```
如果显示 `Done` 或者 `Succcessfully` 字样，那么就安装成功了。

### 安装pip
如果说Python解析器是Python环境的骨头，那么pip（PyPI）就是Python环境的灵魂。但是比较神奇的是，目前没有比较好的直接命令安装pip的方法，只能使用下载脚本在线安装的方法。（我知道你可能会说 `sudo apt install python3-pip`，但是经我的实测，这样安装的pip往往会存在版本过旧、兼容性不佳、权限不足等等问题，因此不建议如此安装）

pip官网建议的安装方法如下，其中链接 `https://mirrors.aliyun.com/pypi/simple/` 可以换成任何速度快的PyPI镜像源地址(下文也是如此），Debian家族、Ubuntu家族在sudo用户组权限下请在第二行命令前面增加 `sudo`。：
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py -i https://mirrors.aliyun.com/pypi/simple/
rm get-pip.py
```
但是很多Linux系统中默认没有安装curl，那么使用wget是更好的替代方案。
```
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py -i https://mirrors.aliyun.com/pypi/simple/
rm get-pip.py
```

随后使用 `pip3 --version` 即可查看到pip的版本。有时候可能会安装到旧的版本，那么我们就需要更新：
```
pip3 install --upgrade pip -i https://mirrors.aliyun.com/pypi/simple/
```

到这一步，基本的Python环境已经搭建完成了。但是还有一个小trick：更换镜像源。Python的PyPI官方源没有中国服务器，因此可能访问缓慢，需要修改源。

如果只是临时修改源，可以将pip安装命令后增加 ` -i https://mirrors.aliyun.com/pypi/simple`。

如果需要永久修改源，可以参考如下步骤：

编辑（没有就新建）~/.pip/pip.conf，添加内容如下：
```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
```
保存并退出即可。

自此，Linux下Python环境的搭建就大功告成了。

### 附录
一些常见的PyPI镜像源地址（替换上文中的`https://mirrors.aliyun.com/pypi/simple/`）：

清华大学开源镜像站：https://pypi.tuna.tsinghua.edu.cn/simple

阿里巴巴开源镜像站：https://mirrors.aliyun.com/pypi/simple/

中国科技大学开源镜像站： https://pypi.mirrors.ustc.edu.cn/simple/

豆瓣网开源镜像站：https://pypi.douban.com/simple/

华为开源镜像站：https://mirrors.huaweicloud.com/repository/pypi/simple

参考链接：
1. https://www.python.org
1. https://pip.pypa.io/
1. https://pip.pypa.io/en/stable/reference/pip_install/
1. https://pip.pypa.io/en/stable/installing/
1. https://developer.aliyun.com/mirror/
