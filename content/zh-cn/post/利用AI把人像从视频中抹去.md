---
title: 利用AI把人像从视频中抹去
date: 2020-05-02
draft: false
author: Futrime
description: 我们常常会遇到这样的尴尬：在拍视频的时候，我们明明只是想要拍摄背景，却总是有各种路人甲路人乙出现在视频中，使得后期小哥抓耳挠腮。这个问题终于被伟大的AI解决了。这篇文章中我将会简单介绍这个技术。
categories:
  - 学术讲解
tags:
  - AI
  - 机器学习
  - 深度学习
  - TensorFlow.js
  - bodypix
---

几个月前Google的工程师Jason Mayes开源了一个Web软件——Real Time Person Removal。在这个软件中，AI将会调用你的手机摄像头拍摄的视频并且把里面所有人像抹除。效果如下：

![74691149-882fce00-5196-11ea-80bc-f1b9cb3ff275.gif][1]

是不是很惊艳？在这里，我给大家分享一下这个软件。

官方Github仓库地址：https://github.com/jasonmayes/Real-Time-Person-Removal

但是由于官方提供的示例在国内访问极其缓慢甚至无法访问，因此我特地进行了修改并且搭建了相应的网站:

我的镜像站： https://fsyz.online/demo/Real-Time-Person-Removal/

给完demo，再讲讲原理。

TensorFlow.js官方开源了一个模型：bodypix，这个模型可以实时地在视频中标记出所有人像。这本来是一个用来定位人像的模型，但是正如《三体》中的「思想钢印」一样，我们给他个正号，就是标记人像，给他个负号，就会自动抹去人像了。

  [1]: https://futrime.gitee.io/image-cdn/2020/03/3801279929.gif
