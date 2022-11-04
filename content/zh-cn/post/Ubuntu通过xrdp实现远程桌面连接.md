---
title: Ubuntu通过xrdp实现远程桌面连接
date: 2022-11-04
draft: false
author: Futrime
description: 对于Windows用户来说，这比VNC方便多了
image: https://s2.loli.net/2022/11/04/znj5Esx8rTXJCPU.png
categories:
  - 经验杂谈
tags:
  - 远程桌面连接
  - Ubuntu
  - xrdp
  - 蓝屏
  - 黑屏
---

首先需要确保Ubuntu已经安装了桌面环境。

输入以下命令安装xrdp：

```bash
sudo apt install xrdp -y
```

可能需要开放3389端口。

最后是一个比较大的坑：在远程登录前，必须将本地登陆的桌面环境用户登出，否则远程桌面会蓝屏或黑屏。xrdp和Windows不同，不会自动将已经登陆的用户注销。