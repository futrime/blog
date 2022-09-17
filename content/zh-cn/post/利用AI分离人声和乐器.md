---
title: 利用AI分离人声和乐器
date: 2020-03-22 00:00:00
draft: false
author: Futrime
description: 最近发现一个很棒的网站，可以把一段音频中的不同乐器和人声给分离出来。这个网站利用AI进行分离，而不是使用传统软件左右声道对比的方式，所以可以应用于任何音频。这篇文章我讲讲使用这个网站的体验。
categories:
  - 搞机记录
  - 经验杂谈
tags:
  - AI
  - 深度学习
  - 分离人声
  - 乐器
  - 音乐
  - 人工智能
---

这个网站的是 [melody ml][1] 。打开这个网站后如下所示：

![Screenshot_2020-03-16 melody ml.png][2]

在下面可以选择分离模式：分离人声和乐器（Vocals + Instrumental ）或者分离所有组分（Vocals + Drums + Instruments + Other）。点击中间的输入框输入自己的电子邮件地址，然后点击`Upload MP3`，选择自己的音频上传即可。等待服务器处理完成后，系统将会自动将音频发送到输入的邮箱。

就效果来说，显然是没有利用立体声分离的效果好，但是这最关键在于可以分离单声道音频。

  [1]: https://melody.ml/
  [2]: https://futrime.gitee.io/image-cdn/2020/03/335496539.png
