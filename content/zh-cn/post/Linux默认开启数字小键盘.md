---
title: Lubuntu默认开启数字小键盘
date: 2020-02-08 12:15:00
draft: false
author: Futrime
description: 想必很多使用Linux桌面系统的用户都会遇到这样一个棘手问题：登录界面的数字小键盘默认是关闭的，下意识输入数字密码的时候总是会出问题。这篇文章我将会简单地讲一下Linux桌面系统下默认开启数字小键盘的方法。
image: https://futrime.gitee.io/image-cdn/2020/02/3423400315.jpg
categories:
  - 搞机记录
  - 经验杂谈
tags:
  - Linux
  - Numlock
  - 数字小键盘
---

在Ubuntu等某些Linux桌面系统中有相关的设置，只要打开就好了。

但是在Linux Mint、Lubuntu、Debian等其他的Linux桌面系统中，相关设置不可用或者根本没有相关设置，此时就需要以下步骤处理：

首先，我们要安装关键软件 `numlockx` 。
```
sudo apt update && sudo apt install numlockx
```
然后 *在root权限或者sudo权限下* 编辑 `/etc/lightdm/lightdm.conf` 文件。在最后新加一行：
```
greeter-setup-script=/usr/bin/numlockx on
```
保存重启即可看到效果。

#### 附：如何在root权限或者sudo权限下编辑？
我们可以直接登陆root账户，或者登录sudo用户组的用户（一般Ubuntu系列的默认用户）后在命令前输入`sudo`即可。
