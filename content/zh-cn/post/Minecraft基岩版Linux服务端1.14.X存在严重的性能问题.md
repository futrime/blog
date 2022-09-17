---
title: Minecraft基岩版Linux服务端1.14.X存在严重的性能问题
date: 2020-04-29 11:47:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/04/393612475.jpg
categories:
  - 搞机记录
  - 经验杂谈
tags:
  - Minecraft
  - 服务器
  - Bug
  - 性能
---

最近在独自搭建的服务器上玩的时候发现总是会卡顿，骑马的时候两步一卡，毫无游戏体验。我使用的服务器是阿里云的ECS（Intel® Xeon® E5-2682v4单核心+1GB RAM），按理说这个性能已经足够了。

于是我就尝试在自己的电脑（Intel® Core™ i7-8750H + 32GB RAM + Samsung® SSD 970 EVO PLUS）上用虚拟机开，分配了4C8T加上16G内存，发现依然很卡。

于是我使用TOP命令查看占用，发现只要有玩家连接，CPU0的占用就达到了100%而其他线程都懒得动。

后来我在[Jira][2]上找到了一个Bug：[BDS-2574][3]，在1.14.X版本的Linux基岩版服务端存在严重的性能问题，目前唯一解决办法就是换成Windows（会不会是Microsoft的阴谋呢/误）。但是毕竟Linux还是更适合作为服务端，希望改进吧。

社区中提到1.14.60版有所改善，根据我的测试，如果把在以前版本的世界导入反而会更加糟糕，而直接用1.14.60创建则有一点点好转，但只是一点点。

参考链接：
1. https://bugs.mojang.com
2. https://bugs.mojang.com/browse/BDS-2574
3. https://blog.futrime.com/hacking/302.html

  [2]: https://bugs.mojang.com
  [3]: https://bugs.mojang.com/browse/BDS-2574
