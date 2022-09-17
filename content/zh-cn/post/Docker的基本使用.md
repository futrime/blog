---
title: Docker的基本使用
date: 2020-06-06
draft: false
author: Futrime
description: Docker的常见操作包括镜像处理、容器处理和存储卷处理，这篇文章我讲讲Docker的基本使用方法和常见命令。
image: https://futrime.gitee.io/image-cdn/2020/04/777977219.png
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Minecraft
  - 服务器
  - Docker
---

如果你还没有安装配置Dockers， 请参考我的前一篇博客：[Docker的安装配置][2]，如果你想看看Docker的实际运用，请参考：[在VPS上搭建Minecraft基岩版服务器][3]、[利用Docker安装Jupyter Notebook/Jupyter Lab并配置sudo权限][4]。

## Docker的独特之处

Docker和虚拟机很大的一个区别在于虚拟化的程度，所有虚拟机，无论是半虚拟化（如OpenVZ）还是全虚拟化（如KVM），都是或多或少地对程序执行进行了虚拟化，但是Docker仅仅对运行环境进行了虚拟。其实Docker上运行的所有软件都是在宿主机运行的，只是有意地和宿主机隔离开不能互相访问内存和文件罢了。也就是说，Docker并不能拿来调试计算机病毒等等，因为Docker内的软件可以直接访问到计算资源，如果病毒利用了CPU等芯片的漏洞的话，宿主机照样会被攻击。

而相比Virtualenv这样的虚拟环境工具，Docker最大的优势在于文件和内存的隔离，纯天然绿色无污染。Docker中跑的程序并不能访问到任何其他环境的内存和文件，也因此你可以用Docker跑多个安装在同一个位置的软件而不需要另外配置。

我们可以用一个通俗易懂的方式来解释Docker：微信小程序，Docker和微信小程序类似，都是直接运行在宿主机（你的手机）上，但是所有数据都被微信自己管理，因此更加纯净、安全。说到这个，现在很多手机厂商都在推的所谓“快应用”其实也是类似原理。

## Docker的工作流程

Docker重要的概念有六个：Docker Daemon、Docker CLI、镜像、容器、仓库、仓库注册服务器（Registry），容我一一道来。

Docker Daemon是Docker中最重要的组件，负责Docker镜像、容器、仓库的管理，可以比作是微信本身（当然他没有社交功能）。

Docker CLI是Docker Daemon和你交互的界面，可以比作是微信下拉小程序菜单以及进入小程序后右上角两个按钮。

镜像相当于还没打开的小程序（也就是那个小程序菜单中的图标），当你点击之后会加载一会儿才能打开。

容器相当于已经打开的小程序，在Android系统中，如果你查看后台任务，你会发现小程序和微信是独立开的，你可以只打开小程序而不打开微信本身（实际上打开了微信内核）。

仓库容纳了已经拉取的镜像，相当于小程序菜单。

Registry相当于微信上存放所有小程序的服务器，当我们需要打开一个没有见过的小程序的时候，微信会自动上去下载下来。

## 常见命令

在这里我按照不同的目的，从开始到结束讲解，而不是把各个命令分别讲解。我将会以Minecraft基岩版服务器为例。

### 我想直接运行一个开箱即用的软件

首先建议使用Docker Hub国内镜像，请参考：[Docker的安装配置][5]。

首先拉取Minecraft基岩版服务器镜像：

```
docker pull itzg/minecraft-bedrock-server
```

漫长的拉取等待结束后，查看镜像是不是成功拉取了：

```
docker images
```

如果出现`itzg/minecraft-bedrock-server`字样，就代表成功了，如图：

![批注 2020-04-29 110153.jpg][6]

然后就可以运行这个镜像了：

```
docker run -itd --name <容器名称> -e EULA=TRUE -e SERVER_NAME=<你的服务器名称> -p <你的服务器端口>:19132/udp itzg/minecraft-bedrock-server
```

其中`-itd`的 it 代表允许交互（也就是以后可以用`docker attach <容器名称>`来进入容器做一番操作）， d 代表后台运行，不会直接让你进入容器里。`--name`代表你自己给容器定义的一个名称，如果不设置，Docker会自动生成一个奇怪的名字。`-e`代表设置环境变量，很多镜像都提供使用环境变量来设置某些参数的功能，具体可以参考这些镜像的文档。`-p`代表把宿主机对外端口映射到容器的对外端口（注意顺序）：<宿主机端口>:<容器端口>，后面的`/udp`说明使用UDP协议，tcp和sctp同理。

我们可以使用`docker logs <容器名称>`来查看容器里面到底输出了什么，当我们看到诸如“Server started”字样后就可以链接Minecraft玩了。

### 这个软件把我的电脑弄卡了，我要限制他的性能

这样的话，我们需要重新启动一个容器了，这次我们更改下命令：

```
docker run -itd -m <最大内存> --memory-reservation <警告内存> --cpu-shares=<CPU共享权重> --cpu-period=10000 --cpu-quota=1000 --name <容器名称> -e EULA=TRUE -e SERVER_NAME=<你的服务器名称> -p <你的服务器端口>:19132/udp itzg/minecraft-bedrock-server
```

* 最大内存可以写500M, 1G, 500000K之类的，对于Minecraft服务器建议最小不小于128M。
* 警告内存是个比较玄学的东西。当内存占用超过警告内存后，Docker会不断地向容器里面发送“内存不足”的信号，企图尽可能回收内存页面文件，不过里面的软件理不理你是另一回事。
* CPU共享权重用于在不同容器间分配CPU资源，默认情况是1024。比如A容器是1024，B容器是4096，那么A容器就可以用20%的CPU（按照所有核心计算），B容器可以用80%。
* CPU占用率 = 每周期可用CPU时间 ÷ CPU计算周期， 大于100%则意味着使用多个vCPU，例如如果我分配8个vCPU那么可以设置为`--cpu-period=1000 --cpu-quota=8000`，这两项取值范围都是[1000, 1000000](也就是1ms~1s)。这个和共享权重相互独立，相当于这个是天花板，而共享权重则是动态分配规则。譬如A容器是1024不限制占用，B容器是4096但是限制占用10%，那么B容器顶天就是10%，而A容器可以跑到90%但不能再往上。

### 我想修改下软件的数据

能修改数据的容器往往会配置好数据卷（Volume），我们使用如下命令查看数据卷位置：

```
docker inspect <容器名字> | grep '/var/lib/docker/volumes/'
```

`docker inspect`是个有趣的命令，可以把容器的各种信息显示出来，有兴趣的话可以看看。得到数据卷位置后（啥也没有？那是因为镜像根本没想让你修改数据）用`cd <数据卷位置>`进入。

然后`ls`看看有哪些数据可以修改（并修改）。

### 软件提供了命令界面，我想进去操作一番

使用刚才提到的`docker attach <容器名字>`即可进入，然后就可以畅快地输入`op <你的游戏ID>`来给自己OP权限了（只是一个例子）。如果你发现输入命令没反应，请确认自己是否在运行`docker run`的时候忘记加`-itd`了（三个字母一个都不能少）。

### 玩腻了，删了吧

首先你要用`docker stop <容器名称>`来停止容器。

你可以用`docker rm <容器名称>`来删除容器。

如果你经常只是停止了容器而没有删除，那么会积累相当多的废容器，此时可以使用`docker container prune`自动清除。

使用`docker inspect <容器名字> | grep '/var/lib/docker/volumes/'`查看容器的数据卷名称，如图：

![批注 2020-04-29 112542.jpg][7]

其中`5df6a580ce24474f3a89a7b41d094fe06810e54c6e3689431e79ec5afceefa3e`就是数据卷名称，你的可能不一样，但是同理。

然后使用`docker volume rm <数据卷名称>`来删除。

如果你忘了删除而积累一大堆数据卷，那么可以用`docker volume prune`自动清除。

参考链接：
1. https://docs.docker.com/
2. https://blog.futrime.com/skills/352.html
3. https://blog.futrime.com/hacking/302.html
4. https://blog.futrime.com/hacking/347.html
5. https://hub.docker.com/r/itzg/minecraft-bedrock-server

  [2]: https://blog.futrime.com/skills/352.html
  [3]: https://blog.futrime.com/hacking/302.html
  [4]: https://blog.futrime.com/hacking/347.html
  [5]: https://blog.futrime.com/skills/352.html
  [6]: https://futrime.gitee.io/image-cdn/2020/04/2071276679.jpg
  [7]: https://futrime.gitee.io/image-cdn/2020/04/1494397968.jpg
