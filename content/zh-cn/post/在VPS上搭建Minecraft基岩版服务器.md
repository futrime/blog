---
title: 在VPS上搭建Minecraft基岩版服务器
date: 2020-05-31 06:39:00
draft: false
author: Futrime
description: 在玩Minecraft的时候，我们会有和好友多人联机的需求。而多人联机有多种方式，我认为其中最好的方式就是自己搭建服务器。在这篇文章中我将会介绍如何使用最简单的方法搭建Minecraft基岩版服务器。
image: https://futrime.gitee.io/image-cdn/2020/04/3985268983.jpg
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - Ubuntu
  - Minecraft
  - 基岩版
  - 服务器
  - Docker
  - 游戏
  - 联机
---

## 使用官方服务器软件全局安装

这是官方默认的安装方法，但是有一些特别要求：首先，要求使用Ubuntu操作系统；其次，要求服务器软件全局运行（可以使用nohup或者screen挂在后台）。如果不想换系统或者不接受全局运行，请跳过本部分并前往下一部分：使用Docker运行。

Minecraft官方提供了基岩版服务器软件，但是位置比较隐秘。我们首先打开Minecraft官网 https://www.minecraft.net ：

![Screenshot_2020-04-12 官方网站.png][2]

然后点击右上角的`SUPPORT`：

![Screenshot_2020-04-12 Home.png][3]

一直往下拉，直到`PLAYING MINECRAFT`专题：

![Screenshot_2020-04-12 Home(1).png][4]

点击 `Dedicated Servers for Minecraft on Bedrock` 。然后往下拉到图中位置：

![Screenshot_2020-04-12 Dedicated Servers for Minecraft on Bedrock.png][5]

点击 `visit the download page here` 的 `here` 。

![Screenshot_2020-04-12 Minecraft Server Download.png][6]

同意协议后点击下载客户端并解压。

为了让我们的服务端程序可以持续运行，我们需要在运行服务端程序前输入 `screen -S mc` 来建立一个screen环境（mc可以用任意字符串替换），然后再输入命令 `LD_LIBRARY_PATH=. ./bedrock_server` 运行。按照程序的提示进行配置，随后正常运行后 **先不要关掉终端，自己用Minecraft基岩版客户端连接上服务器后，在服务端界面输入** `op XXX` **， 其中XXX是你自己的用户名** ，这样就可以设置自己为OP了。

配置完以上步骤后，输入Ctrl+A在输入q即可保存环境，然后就可以推出SSH登陆了。

## 使用Docker运行

上面的步骤太麻烦了，怎么办？

Docker Hub上有一个大神itzg已经为我们打包好了整个Minecraft服务器的Docker镜像，我们搬来就用就可以了。该过程需要安装Docker，如果不懂的，请参考：[Docker的安装配置][7]

安装完Docker后，运行如下命令：

```
docker run -itd -e EULA=TRUE -e SERVER_NAME=<你的服务器名称> -p <你的服务器端口>:19132/udp itzg/minecraft-bedrock-server
```

我们还可以增加更多的配置属性，只要使用 `-e <属性名>=<配置值>` 即可。具体清单请参考： https://minecraft.gamepedia.com/Server.properties#Bedrock_Edition_3

其中所有属性名都要小写改大写，并且把短划线`-`改为下划线`_`。

举个例子（我的服务器配置）：

```
docker run -itd -e EULA=TRUE -e SERVER_NAME=服务器PLUS -e DIFFICULTY=normal -e ALLOW_CHEATS=true -e DEFAULT_PLAYER_PERMISSION_LEVEL=visitor -e MAX_PLAYERS=5 -p 19132:19132/udp itzg/minecraft-bedrock-server
```

会输出一串长长的id号，复制下来。

我们可以运行 `docker logs <id号>`查看服务器状态，大约等待5分钟后，该命令的输出中会出现 `Server started`字样。

随后使用游戏客户端登陆服务器并运行`docker attach <id号>`进入服务器配置，输入`op XXX`（XXX是你自己的用户名）即可把自己设置为OP，然后输入Ctrl+P和Ctrl+Q退出即可。

  [2]: https://futrime.gitee.io/image-cdn/2020/04/839175543.png
  [3]: https://futrime.gitee.io/image-cdn/2020/04/2769552309.png
  [4]: https://futrime.gitee.io/image-cdn/2020/04/176705270.png
  [5]: https://futrime.gitee.io/image-cdn/2020/04/3485814941.png
  [6]: https://futrime.gitee.io/image-cdn/2020/04/3747231590.png
  [7]: https://blog.futrime.com/skills/352.html
