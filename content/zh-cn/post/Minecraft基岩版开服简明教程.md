---
title: Minecraft基岩版开服简明教程
date: 2022-09-12
description: 随着Minecraft的发展和微软的推动，加上如LiteLoaderBDS等第三方工具的开发，基岩版已经具备比肩Java版的拓展性，从而得到越来越多玩家青睐。本文将简要介绍如何使用Minecraft基岩版专用服务器（BDS）开服在Windows平台开服。
draft: false
author: Futrime
image: https://s2.loli.net/2022/09/12/z4ydlSErjqZug1T.webp
categories:
categories:
  - 经验杂谈
tags:
  - Minecraft
  - 基岩版
  - 服务器
---

## 系统要求

* CPU：至少具备两个物理核心的64位Intel或AMD的x86处理器
* 运行内存：至少1GB
* 操作系统：Windows 10 1703或更新；或Windows Server 2016或更新

如果你希望在自己的电脑上同时运行服务端和游戏，你需要在带有管理员权限的PowerShell窗口（在开始菜单中右键PowerShell并选择以管理员身份运行以打开）中输入并执行以下命令：

```powershell
CheckNetIsolation.exe LoopbackExempt -a -p=S-1-15-2-1958404141-86561845-1752920682-3514627264-368642714-62675701-733520436
```

## 安装配置

前往[Minecraft官方下载页面](https://www.minecraft.net/download/server/bedrock)。

![基岩版服务器下载|Minecraft](https://s2.loli.net/2022/09/12/LsMuHrSXj3Ri1p2.png)

在左边的Windows服务端下载板块内，勾选**I agree to the Minecraft End User License Agreement and Privacy Policy**前的选择框，然后点击**下载**。

建立一个新文件夹，将下载得到的压缩包内所有内容解压到该文件夹内。

![解压得到的文件](https://s2.loli.net/2022/09/12/U56aGMWk4KOZedH.png)

运行 `bedrock_server.exe` ，等待初次启动完成。你可能会遇到如下弹窗，点击**允许访问**即可。

![防火墙弹窗](https://s2.loli.net/2022/09/12/tMcNkO3ys1hvqaD.png)

初次启动成功后，输入并执行 `stop` 关闭服务端。用文本编辑器打开 `server.properties` 编辑配置。主要关注以下内容：

```
server-name=Dedicated Server
# 服务器名称，可以是任何不含英文分号的字符串

gamemode=survival
# 新玩家的游戏模式，可以是“survival” 、“creative”或“adventure”

difficulty=easy
# 游戏难度，可以是“peaceful”、“easy”、“normal”或“hard”

level-name=Bedrock level
# 存档名称，如果需要使用自己的存档，就要修改
```

如果需要使用自己的存档，请将导出的 `.mcworld` 存档后缀名改为 `.zip` ，然后在服务端 `worlds` 内建立一个以前面 `level-name` 设置的字符串为名字的文件夹，并将存档压缩包内所有内容解压到这个文件夹。

最后再次运行 `bedrock_server.exe` 即可开服。

## Mod安装

虽然基岩版不支持Java版的Mod，但得益于官方提供的Addon支持，以及第三方插件加载工具（如LiteLoaderBDS）的发展，基岩版已经能够安装大量的拓展了。欲知更多，且听下回分解。