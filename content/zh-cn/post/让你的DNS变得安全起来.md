---
title: 让你的DNS变得安全起来
date: 2020-03-15 00:00:00
draft: false
author: Futrime
description: 随着世界局势的快速发展和网络技术的飞速进步，许多传统的互联网协议逐渐被更为安全的新协议取缔。例如，HTTP协议被HTTPS协议取代，Telnet协议被SSH协议取代，FTP协议（部分地）被SFTP协议取代。但是就目前来说，全世界大部分人所使用的域名解析系统依然使用老旧的DNS协议。这篇文章中我将会简要介绍一下DNS协议的安全替代方法。
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - DNS over HTTPS
  - DNS
  - DNS协议
  - DNSSec
  - DNS-over-TLS
  - 本地DNS服务器
  - 私有DNS
  - 私有
  - 安全
  - 网络安全
  - DNSCrypt
  - Cloudflare
  - 红鱼
  - GeekDNS
  - Firefox
  - Android
  - AdGuard
  - AuroraDNS
  - DNS服务器
  - 信息安全
---

（今天刚好3·15，有点莫名其妙地应景）

### 综述

在[上一篇文章][1]中，我简要谈了我们现在使用的传统DNS协议所面临的问题和挑战。网络通信界的专家们为了完善这套系统以及增强用户的安全性，开发出了DNSSec、DNSCrypt、DNS-over-TLS和DNS-over-HTTPS等协议，其中前两种协议有着自身的局限性，往往更多地用在虚拟专用网络内部的解析中，而很少用在公网。DNS-over-TLS的数据包由于有着明显的特征，也很容易被路由的服务器拦截，因此稳定性并没有DNS-over-HTTPS高。因此我们更多地倾向于使用DNS-over-HTTPS。

目前全世界提供DNS-over-HTTPS服务的大型网络通信企业只有两家：Google和Cloudflare，其中Google不在中国内地提供服务，因此我们能选的就只有Cloudflare了，但是Cloudflare毕竟是海外企业，连接稳定性和可靠性并不太高。除此之外还有很多小型DNS-over-HTTPS服务提供商，例如[红鱼互联网][2]和[GeekDNS][3]，服务的稳定性和可靠性可能没有那么优秀，但是总也比传统的DNS服务要优良，我们不妨也可以选用。

### 配置

我们以Cloudflare的DNS-over-HTTPS服务为例。首先我们访问[Cloudflare的DNS-over-HTTPS服务技术支持页面][4]，可以看到服务的地址是[https://cloudflare-dns.com/dns-query][5]。

假如我们正在使用Firefox浏览器，那么可以直接打开菜单栏——首选项——常规，拉到最下面的网络设置——设置...，在弹出的菜单中拉到最下面可以看到有一栏`启用基于HTTPS的DNS`选择框。勾选即可开启DNS-over-HTTPS服务。Firefox默认使用了Cloudflare服务器，不需要我们另外配置。如果我们使用了私有的服务器或者其他服务商提供的服务，那么就可以选择自定义，输入服务地址即可（注意要带上`https://`前缀）。Cloudflare服务器可能会比较慢，但是可以根据这几篇文章的内容优化速度：
1. [减小Cloudflare DoH到本地的延迟的方法][6]
1. [使用cdn解决Dns Over Https的高延时方案][7]

假如我们正在使用Windows 10，微软官方称将会在未来版本中添加系统对DNS-over-HTTPS服务的支持（[Windows will improve user privacy with DNS over HTTPS][8]），意思就是现在暂时还不支持。所幸Cloudflare提供了相应的客户端支持，请参考：[Running a DNS over HTTPS Client][9]。

假如我们正在使用Android系统。Android官方原版提供了DNS-over-HTTPS的支持（毕竟Android由Google开发，而Google在大力推着它自己的DNS-over-HTTPS），但是很多国内的Android UI并没有提供相关的接口。我们需要下载一个App：[QuickShortcutMaker][10] 来进入。下载安装完QuickShortcutMaker后打开，搜索`更多连接方式`（不同机型不太一样，但是记住应用名下面的灰色小字是
```
com.android.settings/
com.android.settings.Settings$NetworkDashboardActivity
```
即可。点击打开，然后点击`启动`就可以进去设置页面了。在设置页面中选择`私人DNS`——`私人DNS提供商主机名`，输入DNS-over-HTTPS服务地址保存即可使用。保存后不要退出，等待约30秒，如果仍然提示`无法连接`，请确认服务地址是否输入正确。

假如我们正在使用Chrome浏览器，很抱歉，Chrome对于DNS-over-HTTPS服务的支持尚处于实验阶段（[谷歌正在 Chrome 78 中实验 DoH - OSCHINA][11]），我们更建议使用Windows下Cloudflare的客户端解决方案（参考上面）。

### 其它方案
让自己的DNS变得安全起来，除了使用更安全的协议，难道没有别的方法了吗？当然不是！我们还可以使用AuroraDNS、AdGuard Home等软件搭建私有DNS净化过滤系统，请参考：
1. [用AdGuard Home搭建一个无广告和跟踪的公共DNS][12]
1. [AuroraDNS / 简单易用的抗DNS污染工具][13]

当然，我们还可以自己搭建本地DNS服务器！
1. [一键搭建属于自己的dns服务器][14]
1. [利用nano pi搭建dnsmasq加速解析速度][15]

### 结语
当然咯，不管DNS安全与否、稳定与否，最重要的还是我们自己要能够识别出哪些是有问题的网站，哪些是健康的网站。DNS只能为我们提供技术上的保障，而根本的保障就是我们自己的思想意识！

参考链接：
1. https://blog.futrime.com/freetalk/186.html
1. https://developers.cloudflare.com/1.1.1.1/dns-over-https/
1. https://www.rubyfish.cn/
1. https://www.233py.com/
1. https://www.hunyl.com/118.html
1. https://www.hunyl.com/120.html
1. https://www.hunyl.com/117.html
1. https://www.hunyl.com/107.html
1. https://www.hunyl.com/100.html
1. https://www.hunyl.com/64.html


  [1]: https://blog.futrime.com/freetalk/186.html
  [2]: https://www.rubyfish.cn/
  [3]: https://www.233py.com/
  [4]: https://developers.cloudflare.com/1.1.1.1/dns-over-https/request-structure/
  [5]: https://cloudflare-dns.com/dns-query
  [6]: https://www.hunyl.com/117.html
  [7]: https://www.hunyl.com/118.html
  [8]: https://techcommunity.microsoft.com/t5/networking-blog/windows-will-improve-user-privacy-with-dns-over-https/ba-p/1014229
  [9]: https://developers.cloudflare.com/1.1.1.1/dns-over-https/cloudflared-proxy/
  [10]: https://futrime.gitee.io/image-cdn/2020/03/2594956802.zip
  [11]: https://www.oschina.net/news/109780/chrome-78-dnsoverhttps
  [12]: https://www.hunyl.com/120.html
  [13]: https://www.hunyl.com/107.html
  [14]: https://www.hunyl.com/64.html
  [15]: https://www.hunyl.com/100.html
