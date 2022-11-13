---
title: Debian永久禁用Swap
date: 2022-11-13
draft: false
author: Futrime
description: 如果只是运行swapoff，重启后Swap又回来了
image: https://s2.loli.net/2022/11/13/fA7ubr35jYqpE4F.jpg
categories:
  - 经验杂谈
  - 奇技淫巧
tags:
  - Swap
  - Debian
  - 禁用
  - swapoff
---

部分应用，尤其是Kubernetes这样的集群部署环境，是不允许使用Swap的。但Linux在安装的时候，往往会启用Swap分区，此时就需要我们手动永久禁用Swap。一般来说，我们会通过运行`swapoff -a`禁用，然而在Debian中，重启后Swap分区会被重新激活，这导致应用安装会出现问题。此时我们需要以下操作：

首先，找到自动启用Swap的服务：

```sh
systemctl status *swap
```

找到之后，彻底禁用掉：

```sh
systemctl mask <服务名>
```

一般来说，服务名是`dev-*.swap`的形式。