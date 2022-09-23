---
title: Hyper-V安装系统时报错 No operating system was loaded
date: 2020-06-13 12:01:00
draft: false
author: Futrime
description: 最近玩Docker Desktop，要求使用Hyper-V作为后端，而Hyper-V和VMWare Workstation Player或VirtualBox不兼容，只能卸载这些顺手的虚拟机软件，尝试上手Hyper-V。在使安装系统时遇到“No operating system was loaded”的报错，特此记录。
image: https://futrime.gitee.io/image-cdn/2020/05/3997470385.png
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - 操作系统
  - Hyper-V
---

我直接使用快速创建建立了虚拟机，选择使用ISO启动，发现提示如下界面：

![无标题.png][2]

点击Restart也没反应，不得不说Hyper-V对于鼠标的支持真的是玄学。

（顺便一提，之前在Hyper-V上安装Phoenix OS也是用不了鼠标）

捣鼓了好一阵子发现，其实是在出现如下画面的时候，要快速地按任意键：

![无标题.png][3]

这个画面出现的时间很短（1s不到），所以要保持留意。

  [2]: https://futrime.gitee.io/image-cdn/2020/05/2708408829.png
  [3]: https://futrime.gitee.io/image-cdn/2020/05/2267788041.png
