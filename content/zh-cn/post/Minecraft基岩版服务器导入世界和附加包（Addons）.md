---
title: Minecraft基岩版服务器导入世界和附加包（Addons）
date: 2020-05-09 08:30:00
draft: false
author: Futrime
description: Minecraft官方提供了基岩版专用服务器（Bedrock Dedicated Server, BDS）供玩家进行简单的开服，但是官方教程和FAQ中并没有提到如何将外部世界或附加包导入服务器中，这为想要在服务器上玩别人的地图的玩家带来不便。在这篇文章中，我简单介绍一下如何在基岩版官方服务器中导入世界和附加包。
image: https://futrime.gitee.io/image-cdn/2020/04/4181545536.jpg
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - Minecraft
  - 基岩版
  - 服务器
  - 附加包
  - Addons
  - 导出
---

## 首先你需要正确配置基岩版专用服务器
不懂的请看我前一篇博客： https://blog.futrime.com/hacking/302.html 。

## 第一步：导入/修改世界

如果你下载到了后缀名为 `.mcworld` 的世界文件或者世界文件压缩包且希望直接玩，或者你的世界，那么就可以直接跳到第三步。如果你已经导入了世界，可以直接跳到第二步。

如果你希望修改世界，请在自己的客户端进行如下操作：

点击如图所示绿色按钮

![批注 2020-04-21 143300.jpg][2]

在弹出的界面中选择你的世界文件即可。

随后你就可以进入世界中进行一系列修改。

## 第二步：导出世界

进入世界编辑界面，点击`游戏`，拉到最下方：

![批注 2020-04-21 143624.jpg][3]

选择`导出世界`，选择导出位置并点击确定即可。

## 第三步：将世界导入到服务器

首先我们需要将后缀名为`.mcworld`的世界文件的后缀名改为`.zip`，随后需要在BDS服务端的`/worlds`目录中建立一个与导出的世界名字相同的新目录。比如刚才导出的世界文件是`xxx.mcworld`，那么就建立目录`xxx`：

![批注 2020-04-21 144319.jpg][4]

然后将zip文件上传并解压到BDS的`/worlds/xxx`文件夹中，确保存在`/worlds/xxx/levelname.txt`：

![批注 2020-04-21 144401.jpg][5]

然后编辑BDS的`/server.properties`文件，定位到`level-name=`处，修改为`level-name=xxx`。

## 第四步：导入附加包

如果世界没有附加包，可以直接启动服务器。

BDS默认情况下并不会读取世界文件中的资源包和附加包，我们需要将`/worlds/xxx/behavior_packs`中的所有文件拷贝到`/behavior_packs`，并且将`/worlds/xxx/resource_packs`中的所有文件拷贝到`/resource_packs`中。

默认情况下，服务器不会强制客户端加载材质包，如果需要强制加载，编辑`/server.properties`文件：

定位到 `texturepack-required=`处并改为`texturepack-required=true`。

随后启动服务器即可。

  [2]: https://futrime.gitee.io/image-cdn/2020/04/2990321552.jpg
  [3]: https://futrime.gitee.io/image-cdn/2020/04/4165206457.jpg
  [4]: https://futrime.gitee.io/image-cdn/2020/04/3393054863.jpg
  [5]: https://futrime.gitee.io/image-cdn/2020/04/902714268.jpg
