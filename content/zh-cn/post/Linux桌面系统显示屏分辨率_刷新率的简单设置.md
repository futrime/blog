---
title: Linux桌面系统显示屏分辨率/刷新率的简单设置
date: 2020-02-07
draft: false
author: Futrime
description: 在Linux的桌面系统中，有时候我们会遇到系统不能正确识别显示器导致分辨率和刷新率异常，本篇博客我将简要介绍Linux下设置显示器分辨率和刷新率的方法。
image: https://futrime.gitee.io/image-cdn/2020/02/3933570053.jpg
categories:
  - 搞机记录
  - 经验杂谈
tags:
  - Linux
  - 桌面
  - 显示屏
  - 分辨率
  - 刷新率
---

Linux分辨率和刷新率不正确分为以下两种情况：
1. 可以正确识别可用分辨率
2. 无法正确识别分辨率

第一种情况下，我们可以在系统设置——显示器设置中查看系统识别到的分辨率，如果有符合自己显示器的就可以选择应用。

第二种情况下，或者第一种情况中没有适合的选项时候，我们就需要自己编写配置。我们首先需要知道自己显示器支持的分辨率和刷新率。如果不知道的可以前往显示器官网或者 http://detail.zol.com.cn 根据型号查看。具体操作流程如下：

1. 计算配置参数

我们使用`gtf`命令计算所需要的参数，方法如下：
    gtf <列数> <行数> <刷新率>
例如我的显示器是 1440x900 60Hz，我就输入如下：
    gtf 1440 900 60
返回类似如下：
      # 1440x900 @ 60.00 Hz (GTF) hsync: 55.92 kHz; pclk: 106.47 MHz
      Modeline "1440x900_60.00"  106.47  1440 1520 1672 1904  900 901 904 932  -HSync +Vsync
其中`Modeline`开头的一行就是计算出来的参数。

2. 将配置应用到显示屏上

命令如下：
    xrandr --newmode <参数>
    xrandr --newmode "1440x900_60.00"  106.47  1440 1520 1672 1904  900 901 904 932  -HSync +Vsync
其中 `<参数>` 是我们在上一步中计算出来的参数`Modeline`后面的部分。

随后我们使用 `xrandr` 命令查看我们对应的显示器，以我的为例，命令返回如下字样：

```
Screen 0: minimum 320 x 200, current 1440 x 900, maximum 8192 x 8192
VGA-1 connected primary 1440x900+0+0 (normal left inverted right x axis y axis) 0mm x 0mm
   1024x768      60.00  
   800x600       60.32    56.25  
   848x480       60.00  
   640x480       59.94  
   1440x900_60.00  60.00*
HDMI-1 disconnected (normal left inverted right x axis y axis)
HDMI-2 disconnected (normal left inverted right x axis y axis)
DP-1 disconnected (normal left inverted right x axis y axis)
DP-2 disconnected (normal left inverted right x axis y axis)
```

其中 `VGA-1` 就是我正在使用的显示器。输入以下命令将配置添加到显示器上：
    xrandr --addmode <显示器代号> <配置名>
其中显示器代号就是上面的 `VGA-1` ，配置名就是计算出来的Modeline后面的双引号部分（带引号），例如：
    xrandr --addmode VGA-1 "1440x900_60.00"
到这一步，显示器配置就可以说是成功地完成了，接下来要应用配置。

我们有两种方法：
1. 使用如下命令
```
xrandr --output <显示器代号> --mode <配置名>
```
例如：
```
xrandr --output VGA-1 --mode "1440x900_60.00"
```
但是万一配置有误导致无法显示，我们只能重启恢复的，因此建议大家使用以下方法：

2. 打开设置——显示器设置，选择应用刚才添加的配置。

在这种操作模式下，大部分设置软件都会有计时器，假如配置失败会在一定时间内或者按下`Esc`键自动恢复。

到这一步，我们的设置就基本完成了，但是还存在一个问题，那就是这个设置会在重启后消失，重新配置比较麻烦。因此我们将以上步骤写在一个`.sh`文件中：
```shell
xrandr --newmode <参数>
xrandr --addmode <显示器代号> <配置名>
xrandr --output <显示器代号> --mode <配置名>
```
使用 `chmod +x <文件名>.sh` 赋予可执行权限。

以后每次开机登录后直接运行这个 `.sh` 文件就可以了。

注意！一定要确保这个模式在你的当前显示器上可用后再进行这些操作（否则就只能重启了）。
