---
title: ROS无法安装的解决方案
date: 2022-12-13
draft: false
author: Futrime
description: ROS的安装确实令人头疼
image: https://i.ibb.co/6rwHV0F/fittosize-752-0-ac18888d46a5284d670a750269e86e26-f-fts-dualarm-manipulator-sps-2018-mmerz-015-2019-0.jpg
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - ROS
---

有时候安装ROS会出现如下报错：

```
The following packages have unmet dependencies:
 ros-noetic-desktop-full : Depends: ros-noetic-desktop but it is not going to be installed
                           Depends: ros-noetic-perception but it is not going to be installed
                           Depends: ros-noetic-simulators but it is not going to be installed
                           Depends: ros-noetic-urdf-sim-tutorial but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

一些文章提出解决方案是运行如下命令：

```bash
sudo aptitude install ros-noetic-desktop-full
```

然后根据命令提示进行冲突排解，但这未必能够成功安装。

今天安装发现，这可能和**build-essential**依赖的某些库冲突，所以需要卸载`build-essential`，并在装完ROS后重新安装。

```bash
sudo apt remove build-essential
```