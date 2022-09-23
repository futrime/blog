---
title: Linux下持久化设置显示屏分辨率/刷新率
date: 2020-04-12 00:00:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/02/720154920.png
description: 我试了不少方法，终于找到了Linux下持久设置显示屏分辨率的方法了，解决了两个多月前遇到的问题。
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - Linux
  - 桌面
  - 显示屏
  - 分辨率
  - 刷新率
  - 持久化
---

在两个多月前，我发了一篇博客[《Linux桌面系统显示屏分辨率/刷新率的简单设置》][2]，讲了一下Linux桌面环境下使用`xrandr`设置分辨率和刷新率的方法，但是却存在一个重大缺陷：无法自动设置。

我尝试了在`~/.bashrc`文件中设置自启动，但是却发现这个自启动是在终端（终端模拟器）中登录的时候自动调用的，也就是说如果不使用`Ctrl+Shift+T`打开终端模拟器就不会执行。再说了，在终端模拟器中，显示器也是模拟的，完全没有办法设置。

网上大部分方法都是修改`/etc/X11/xorg.conf`文件。但是我看到似乎要修改很多行，而且这个文件是X Window的关键配置文件，修改错误了就会凉凉了。所以一直没有采用，之前的文章也没有写出来。

后来我看到了这篇文章：[《Ubuntu16.04调整屏幕分辨率至1920×1080》][3]，在不起眼处找到了解决方法：我们只需要在`/etc/profile`中追加上次输入的命令即可：
```
xrandr --newmode <参数>
xrandr --addmode <显示器代号> <配置名>
xrandr --output <显示器代号> --mode <配置名>
```
重启就能发现，已经自动设置好了。这个方法安全简单，就算真的出问题了直接用SSH连接或者用U盘启动盘启动（甚至直接在GRUB里面操作都可以），修改回`/etc/profile`文件就能恢复了。

参考链接：
1. https://blog.futrime.com/hacking/116.html
1. https://blog.csdn.net/killerstranger/article/details/80559914

  [2]: https://blog.futrime.com/hacking/116.html
  [3]: https://blog.csdn.net/killerstranger/article/details/80559914
