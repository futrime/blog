---
author: Futrime
title: 自建KMS服务器激活Windows
description: 如果不想承受高昂的正版Windows购买费用，也不想承受各类AutoKMS一键激活工具可能携带威胁程序的风险，那么何不自己搭建激活服务器呢？
date: 2021-01-31T05:31:42Z
categories:
    - 经验杂谈
    - 奇技淫巧
tags:
    - KMS
    - Windows
    - 激活
    - 服务器
image: https://i.loli.net/2021/01/31/zyxtkNjADvIYla4.jpg
draft: false
---

本文不鼓励使用盗版软件的行为，如果您在商业领域使用Windows或者具有经济条件，强烈建议您前往官网购买正版Windows以避免法律纠纷，获得最好的软件体验。

事实上，您应当要知道，安装了Windows 10后，即使没有正版许可证也是能够正常使用的。唯二的代价是：（一）右下角常驻的的激活提示和弹出激活提示；（二）无法修改部分设置，主要是个性化设置。因此，是否需要采用此方法使用盗版激活的Windows，请斟酌损益。

---

众所周知，正版Windows 10是十分昂贵的：

![正版Windows价格](https://i.loli.net/2021/01/31/iwnuWe6xCBTpbmJ.png)

而对于由于硬件性能或者软件兼容性而不得不使用旧版本Windows的用户来说，其使用的Windows版本大多已经终止支持：

[![Windows 7停止支持](https://i.loli.net/2021/01/31/jeQBYNZaoc31giO.png)](https://www.microsoft.com/zh-cn/windows/windows-7-end-of-life-support-information)

在搜索引擎中，可以查询到各种各样的离线KMS激活工具，包括KMSPico、AutoKMS、KMS Activator等等。但是这一类工具往往需要申请管理员权限，在相互转载内容的互联网国情下，我们难以溯本追因，寻找到工具最初发布的地方；又或者这个发布的地方早就已经下线了。而如果在各种下载站点下载，我们又无法保证这个工具是否被打包了其它各类威胁软件。因此使用这类软件的用户，就不得不承担自己的隐私数据有可能被泄露的风险。

在淘宝、京东等电商上搜索 `Windows密钥` 等关键字可以找到大量的价格仅有几元钱的所谓“正版Windows激活码”：

![淘宝搜索](https://i.loli.net/2021/01/31/sPzy1TvVGFixwYL.png)

三四年前，我也常常使用这种方式激活我的Windows，但是后来发现，这类激活方式往往存在激活时间有限的问题；并且因为这类密钥往往来自企业批量购买流出，部分企业会采取对于员工激活的系统进行一定的审查的策略，所以用户的计算机有可能会被访问到，因此也并不太安全。具体请见[5块钱的win10激活码能用么？淘宝密钥背后的秘密：盗版系统有什么危害？](https://www.bilibili.com/video/BV1Bt411Y7wd)。

在如此多的方案都存在各种各样的问题的情况下，到底有没有什么方案是比较好用的呢？

有，那就是使用开源软件，自己搭建KMS服务器。由于是开源软件，有着一定的社区基础，可以排除具有威胁软件的可能。另外因为是自己搭建服务器，可以保证稳定性。

目前来说，在GitHub上，实现这种功能的软件中，社群基础最大的是 [py-kms](https://github.com/SystemRage/py-kms).

安全起见，强烈建议使用Docker方式配置，如果还不会使用Docker，请参见：[Docker的安装配置](https://blog.futrime.com/zh-cn/p/docker%E7%9A%84%E5%AE%89%E8%A3%85%E9%85%8D%E7%BD%AE/)、[Docker的基本使用](https://blog.futrime.com/zh-cn/p/docker%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/)。

其它安装方式也受到支持，详见[Getting Started](https://py-kms.readthedocs.io/en/latest/Getting%20Started.html).

以Docker为例，我们只需要一行命令：

```bash
docker run -d --restart always -p 1688:1688 pykmsorg/py-kms
```

随后在需要激活的Windows系统中进行以下操作。由于所有操作都是使用Windows自带命令，因此十分安全。

首先，在[GLVK Keys](https://py-kms.readthedocs.io/en/latest/Keys.html)查询自己需要的许可证密钥。以Windows 10家庭中文版为例，是 `7HNRX-D7KGG-3K4RQ-4WPJ4-YTDFH` .

然后以管理员模式打开Powershell或命令提示符，在Windows 10中，右键开始图标，选择 `Windows Powershell(管理员)` 即可。

在弹出的窗口中依次运行以下命令：

```sh
slmgr /upk
```

```sh
slmgr /ipk <刚才查询到的许可证密钥>
```

```sh
slmgr /skms <搭建KMS服务的服务器的IP地址，本机则是127.0.0.1>
```

```sh
slmgr /ato
```

随后激活就完成了，会有相应提示。如果提示无法联系到服务器，请尝试ping一下服务器，确认是否可以访问。另外，不建议在本机搭建，因为Windows 10在某些情况下，访问本机会有问题。

参考链接：

1. https://github.com/SystemRage/py-kms
1. https://py-kms.readthedocs.io