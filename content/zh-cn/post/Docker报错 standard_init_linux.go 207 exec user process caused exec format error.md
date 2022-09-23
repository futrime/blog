---
title: "Docker报错 standard_init_linux.go:207: exec user process caused exec format error"
date: 2020-06-14
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/05/2123884079.png
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - Docker
  - 跨平台
  - ARM
---

近日在NanoPi NEO上面尝试运行某个镜像的时候报错如下：

```
standard_init_linux.go:207: exec user process caused "exec format error"
```

报错信息可能不尽相同（随着Docker版本不同有所不同），但是十分相似。

这是因为运行的镜像存在某些层没有在相应CPU架构上编译。这常常在各种arm开发板上或32位操作系统上见到，因为有很多镜像都是只适用于x86-64架构。
