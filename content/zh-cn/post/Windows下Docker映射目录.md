---
title: Windows下Docker Desktop映射目录
date: 2022-12-22
draft: false
author: Futrime
description: 直接挂载是会失败的。。。
image: https://i.ibb.co/StrXDMv/251bdad12c2efc2baba3e8b024caba25-5794984506704759323.jpg
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Windows
  - Docker
  - Docker Desktop
  - 挂载
---

如果在Windows上安装Docker Desktop，按照Linux上Docker Engine的命令进行挂载时，会发现挂载的目标为空。

如果使用`$pwd.path`输入Windows下的完整路径，依然为空。

如果使用`/mnt`开头的WSL内路径，依然为空。

最终解决方案是使用`//<盘符>/<路径>`进行挂载。