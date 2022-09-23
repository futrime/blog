---
title: 利用Docker安装Jupyter notebook/Jupyter Lab
date: 2020-05-30 05:25:00
draft: false
author: Futrime
description: 利用Docker安装Jupyter Notebook/Jupyter Lab可以有效地避免环境冲突和提高安全性。在这篇文章中我就简单介绍如何使用Docker安装Jupyter Notebook/Jupyter Lab并配置sudo权限。
image: https://futrime.gitee.io/image-cdn/2020/04/3984197395.png
categories:
  - 玩机日志
  - 经验杂谈
tags:
---

以下我将Jupyter Notebook/Jupyter Lab统一简称为Jupyter。

## 为什么要用Docker？

使用Docker安装Jupyter有如下优势：

1. Jupyter的环境文件管理可以说是一团糟，到处都有文件，卸载又卸载不全，尤其是当我们想要搭建多个Jupyter环境的时候，更是不得不去配置Jupyter Hub，徒增学习成本。而使用Docker可以科学地管理这些文件，还能追踪修改。

2. 在使用Jupyter的过程中我们往往需要安装一些额外的库，而这些库的安装往往需要sudo/root权限，这意味着我们不得不多次进入宿主机终端进行安装，或者冒着安全风险给予Jupyter Notebook sudo/root权限。而使用Docker可以在容器中授予sudo/root权限而不影响宿主机。

3. 使用Docker，在进行Jupyter项目备份的时候只需要简单地commit即可，不需要到处找文件。

4. 还有很多，基本上都是Docker相比直接装软件的优势。

## 怎么用？

首先你需要安装好Docker，不懂的请参考：[Docker的安装配置][2]。

Jupyter官方提供了多个Jupyter,如图所示：

![批注 2020-04-29 141324.jpg][3]

大家可以根据自己的需要选择，个人建议最低使用scipy-notebook而不是base-notebook或minimal-notebook，因为这两个notebook缺组件。Jupyter还提供了一些优质的非官方notebook，例如支持NVIDIA GPU加速的datascience-notebook。想要了解更多，请参考：[Selecting an Image][4]。在本文中，我将以scipy-notebook为例介绍Jupyter的安装。

我们可以使用一键命令拉取并运行：
```
docker run -d -p <外网端口号>:8888 -e GRANT_SUDO=yes -e JUPYTER_ENABLE_LAB=yes -e RESTARTABLE=yes --user root --mount source=jupyter-workspace,destination=/home/jovyan/ jupyter/scipy-notebook
```

* 不建议加上`-it`选项来进行交互，因为Jupyter可以使用Web终端进行相应配置。
* `-e GRANT_SUDO=yes`和`--user root`说明允许Jupyter的Web终端使用sudo权限，在需要另外安装插件或者软件的时候必须使用，但是如果你的Jupyter是开放给公共使用的，那么建议删除以增加安全性。
* `-e JUPYTER_ENABLE_LAB=yes`说明开启Jupyter Lab（在这里你就知道为什么我总是说Jupyter而不说后面的Notebook/Lab了），如果你需要体验Jupyter Lab的强大功能请打开，但是Jupyter Lab现阶段存在带宽占用很大（1M带宽要打开Jupyter Lab需要等待将近3分钟才能加载完）以及插件不完善的问题，如果不想使用可以关掉以节省资源。值得一提的是，无论是否启用Jupyter Lab，Jupyter Notebook都可以通过`<目的地址>/tree`来访问。
* `--mount source=jupyter-workspace,destination=/home/jovyan/`的目的是让在Jupyter中创建的Notebook可以保留在数据卷中，未来可以通过`/var/lib/docker/volumes/jupyter-workspace/_data`访问，当然，“jupyter-workspace”可以改为任何你喜欢的名字。

Jupyter是用来跑程序的，不可避免会遇到程序Bug导致无限占用，此时如果没有相应的限制，那么很容易导致宿主机崩溃，因此我们可以使用如下命令来运行Jupyter（其他介绍同上）：

```
docker run -m <最大内存> --cpu-period=<CPU计算周期> --cpu-quota=<每周期可用CPU时间> -d -p <外网端口号>:8888 -e GRANT_SUDO=yes -e JUPYTER_ENABLE_LAB=yes -e RESTARTABLE=yes --user root --mount source=jupyter-workspace,destination=/home/jovyan/ jupyter/scipy-notebook
```

* 最大内存可以写500M, 1G, 500000K之类的，建议最小不小于128M。
* CPU占用率 = 每周期可用CPU时间 ÷ CPU计算周期， 大于100%则意味着使用多个vCPU，例如如果我分配8个vCPU那么可以设置为`--cpu-period=1000 --cpu-quota=8000`，这两项取值范围都是[1000, 1000000](也就是1ms~1s)。

如果需要使用HTTPS，可以再加上一点，如下：

```
docker run -m <最大内存> --cpu-period=<CPU计算周期> --cpu-quota=<每周期可用CPU时间> -d -p 8888:8888 -e GRANT_SUDO=yes -e JUPYTER_ENABLE_LAB=yes -e RESTARTABLE=yes --user root --mount source=jupyter-workspace,destination=/home/jovyan/ -mount source=<SSL证书存放地址>,destination=/etc/ssl/notebook jupyter/scipy-notebook start-notebook.sh --NotebookApp.keyfile=/etc/ssl/notebook/<证书KEY文件名> --NotebookApp.certfile=/etc/ssl/notebook/<证书CRT文件名>
```

需要在宿主机找个地方先放着KEY和CRT证书。

参考链接：
1. https://jupyter-docker-stacks.readthedocs.io/en/latest/using/common.html
2. https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html
3. https://docs.docker.com/engine/reference/commandline/run/
4. https://blog.futrime.com/skills/352.html

  [2]: https://blog.futrime.com/skills/352.html
  [3]: https://futrime.gitee.io/image-cdn/2020/04/2649576696.jpg
  [4]: https://jupyter-docker-stacks.readthedocs.io/en/latest/using/selecting.html#community-stacks
