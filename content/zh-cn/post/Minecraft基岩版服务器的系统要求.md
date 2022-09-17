---
title: Minecraft基岩版服务器的系统要求
date: 2020-06-21
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/05/882190216.png
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Minecraft
  - 基岩版
  - 系统要求
  - 硬件配置
  - BDS
---

## 硬件配置

官方要求的配置是：
* Intel或AMD的64位处理器
* 大于等于2个核心
* 大于等于1GB内存

根据实际体验，就算是在J3455的单核心上都能够流畅运行，但是根据[Minecraft漏洞维护器BDS-2574][2]，Linux服务端有空跑CPU以及无法使用超过4个线程的可能，因此使用Linux服务器可能需要更强的配置（反正阿里云ECS跑不了）。

内存方面，开生存地图大概占用100M，小游戏地图可能最多500M，因此Linux下512M内存也是足够的（Windows系统占用多，最好还是上1G）。

## 系统要求

官方给的要求是：
* Windows 10 1703或更高 或
* Windows Server 2016 或
* Windows Server 2019 或
* Ubuntu 18
* 如果使用Windows系统，需要安装[Visual C++ Redistributable Packages for Visual Studio 2015][3]，否则会提示缺少dll，而且如果需要同时运行客户端和服务端，必须要在Powershell中以管理员权限运行`CheckNetIsolation.exe LoopbackExempt –a –p=S-1-15-2-1958404141-86561845-1752920682-3514627264-368642714-62675701-733520436`解除Loopback限制（就算是虚拟机也要）。

根据自己的实践经验，Windows Server 2012 R2或更低并不能正常运行，会出现包括但不限于以下提示：

* Chakra.dll缺失
* api-ms-win-core-memory-l1-1-5.dll缺失
* GetCurrentPackageFamilyName缺失

至于Linux方面，虽然官方只给出了Ubuntu 18，但是Ubuntu 19或Ubuntu 20应该是可以使用的（没试过）。根据[itzg/minecraft-bedrock-server的Dockerfile][4]，Debian 10安装了jq后可以正常运行（没安装可能也可以）。根据实测，CentOS无法正常运行，会提示缺少libssl.so，即使安装了openssl也不行。个人推测只要是正常的基于Debian的Linux发行版都可以。

参考链接：
1. https://www.reddit.com/user/ProfessorValko/comments/9f438p/bedrock_dedicated_server_tutorial/
1. https://bugs.mojang.com/browse/BDS-2574
1. https://www.microsoft.com/en-us/download/details.aspx?id=48145
1. https://hub.docker.com/r/itzg/minecraft-bedrock-server/dockerfile

  [2]: https://bugs.mojang.com/browse/BDS-2574
  [3]: https://www.microsoft.com/en-us/download/details.aspx?id=48145
  [4]: https://hub.docker.com/r/itzg/minecraft-bedrock-server/dockerfile
