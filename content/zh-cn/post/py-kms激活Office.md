---
author: Futrime
title: py-kms激活Office
description: py-kms不仅可以用于激活Windows，还可以激活Office
date: 2021-02-10T23:34:48Z
categories:
    - 经验杂谈
    - 奇技淫巧
tags:
    - KMS
    - Office
    - 激活
image: https://i.loli.net/2021/02/11/mCKWdzSLH6l7agG.jpg
draft: false
---

在我的 [上一篇博客](https://blog.futrime.com/zh-cn/p/%E8%87%AA%E5%BB%BAkms%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%BF%80%E6%B4%BBwindows/) 中，我提到可以自己搭建KMS服务器激活Windows。实际上，不仅Windows，Microsoft Office系列软件也是支持KMS的，因此，我们也可以使用py-kms实现激活Office.

首先要确认自己安装的Office是否是VL版本。在 [Office Tools Plus](https://otp.landian.vip/) 部署安装时，可以选择安装带有“批量版（VL）”的版本。

依然是前往 [官方文档](https://py-kms.readthedocs.io/en/latest/Keys.html#office) 查找需要的许可证密钥。

随后的步骤会比较麻烦，首先打开Windows Powershell（管理员），然后使用cd命令定位到Office文件夹中的OfficeXX文件夹中（其中XX=14代表Office 2010，XX=15代表Office 2013，XX=16代表Office 2016或2019），以我的为例：

```sh
cd 'C:\Program Files\Microsoft Office\Office16\'
```

由于需要管理员权限，不能在文件资源管理器中使用Shift+右键来打开Powershell，但是可以先找到这个位置，复制进入Powershell。记得加上前后引号。

先查询下激活状态，如果激活了就不用折腾了：

```sh
cscript ospp.vbs /dstatus
```

重点看LICENSE STATUS栏，如果是 `---OOB_GRACE---` 就是没有激活；如果是 `---LICENSED---` 就意味着已经激活了。

使用以下命令修改KMS服务器，不要漏了cscript：

```sh
cscript ospp.vbs /sethst:<不带端口的服务器地址>

cscript ospp.vbs /setprt:<端口号>
```

可以再使用 `/dstatus` 查询一下激活状态以及KMS服务器设置是否成功。

然后安装密钥：

```sh
cscript ospp.vbs /inpkey:<密钥>
```

有个有趣的地方，在Office Tools Plus部署的Office 2019标准版批量版的默认密钥就是py-kms文档给的那个，不需要修改。

最后激活

```
cscript ospp.vbs /act
```

再使用 `/dstatus` 看下激活状态。

根据py-kms文档，一次激活只能维持180天，如果需要持续激活，必须保持KMS服务器不掉线。

参考链接：

1. https://github.com/SystemRage/py-kms
1. https://py-kms.readthedocs.io
1. https://otp.landian.vip/