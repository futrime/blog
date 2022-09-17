---
title: Github访问异常解决方案
date: 2020-04-19 00:00:00
draft: false
description: 近日，Github的内容源站服务器访问突然变慢，甚至部分内容无法正常显示。这篇文章中我将会介绍一个简单的解决方案。
author: Futrime
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - tcpdump
  - Github
  - 访问异常
  - 代理
  - 反向代理
  - Github源站
  - 宝塔
  - AppNode
  - hosts
  - 拦截
  - traceroute
  - RESET
---

### 原因分析
近日包括Github在内的部分大型海外网站在国内访问时发现存在版式错误、内容无法加载等现象，以至于无法正常使用。（我知道你们要说啥）我对这个现象进行了一定的分析。

首先我使用浏览器开发者工具中的网络检查功能，发现Github有多个内容源站无法正常加载，它们是：
```
raw.githubusercontent.com
camo.githubusercontent.com
avatars0.githubusercontent.com
avatars1.githubusercontent.com
avatars2.githubusercontent.com
avatars3.githubusercontent.com
```
经过在 [IPAddress.com][1] 的解析测试，我发现这些内容源站都位于 Amazon AWS 国际服务器中。根据我的经验，Amazon AWS 的很多国际服务器机房与中国大陆的网络连接存在带宽小、延迟大以及不稳定等问题，我怀疑是否是这个原因。

我使用在本地查询DNS后在我租用的一台海外服务器上解析，发现DNS解析得到的IP地址是正常的，说明不存在DNS劫持。
```
nslookup raw.githubusercontent.com
Server:		127.0.1.1
Address:	127.0.1.1#53

Non-authoritative answer:
raw.githubusercontent.com	canonical name = github.map.fastly.net.
Name:	github.map.fastly.net
Address: 151.101.228.133
```

但是当我尝试访问的时候，却发现连接被拒绝：
```
wget https://raw.githubusercontent.com
--2020-02-18 08:16:04--  https://raw.githubusercontent.com/
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.228.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.228.133|:443... failed: Connection refused.
```

我使用`tcpdump`获取数据包并分析，捕获的数据包如下：
```
04:05:11.408244 IP (tos 0x0, ttl 64, id 37447, offset 0, flags [DF], proto TCP (6), length 60)
    192.168.1.11.47860 > 151.101.228.133.https: Flags [S], cksum 0x3dcd (incorrect -> 0x2520), seq 452588620, win 29200, options [mss 1460,sackOK,TS val 878015207 ecr 0,nop,wscale 6], length 0
04:05:11.425814 IP (tos 0x0, ttl 80, id 19265, offset 0, flags [DF], proto TCP (6), length 40)
    151.101.228.133.https > 192.168.1.11.47860: Flags [R.], cksum 0x9737 (correct), seq 0, ack 452588621, win 3844, length 0
```
可以看到在我尝试进行TCP连接时，对方发送了一个`RESET FLAG`。但是我再尝试在海外服务器上连接时，并没有这样的事情发生。可能是在我和Github源站之间有某一台服务器拦截了我的请求。

我再尝试使用`traceroute`工具分析数据包路径，结果如下：
```
sudo traceroute -T --port=443 raw.githubusercontent.com
traceroute to raw.githubusercontent.com (151.101.228.133), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  0.507 ms  0.545 ms  0.536 ms
 2  113.70.36.1 (113.70.36.1)  1.836 ms  2.852 ms  3.116 ms
 3  113.98.153.53 (113.98.153.53)  4.566 ms * *
 4  * * *
 5  151.101.228.133 (151.101.228.133)  18.860 ms  19.351 ms  18.970 ms

sudo traceroute -T --port=443 github.com
traceroute to github.com (52.74.223.119), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  0.502 ms  0.494 ms  0.493 ms
 2  113.70.36.1 (113.70.36.1)  2.137 ms  2.875 ms  2.882 ms
 3  * * 197.105.128.219.broad.fs.gd.dynamic.163data.com.cn (219.128.105.197)  2.933 ms
 4  * * *
 5  * * *
 6  * * 202.97.12.41 (202.97.12.41)  19.704 ms
 7  * * *
 8  * 183.91.56.82 (183.91.56.82)  189.465 ms  188.755 ms
 9  * * *
10  * * *
11  52.93.10.1 (52.93.10.1)  188.612 ms * *
12  * 52.93.11.37 (52.93.11.37)  190.486 ms 52.93.11.45 (52.93.11.45)  188.761 ms
13  * 52.93.11.40 (52.93.11.40)  193.483 ms *
14  * * *
15  203.83.223.21 (203.83.223.21)  197.290 ms *  196.403 ms
16  * * *
17  * * *
18  * * *
19  * * *
20  * * *
21  ec2-52-74-223-119.ap-southeast-1.compute.amazonaws.com (52.74.223.119)  195.088 ms  196.269 ms *
22  * ec2-52-74-223-119.ap-southeast-1.compute.amazonaws.com (52.74.223.119)  194.705 ms *
23  ec2-52-74-223-119.ap-southeast-1.compute.amazonaws.com (52.74.223.119)  195.242 ms  194.658 ms  197.461 ms
```
理论上Github和Github源站应该都在美国，那么它们的响应时间不应该有太大差异。而Github源站服务器的响应时间竟然只有约18ms，这个速度比大多数国内机房都要快。只能说明一个事：我的网络服务商的一台路由服务器伪装并拦截了我的请求。

这是在访问某些海外网站经常会出现的事情。

### 解决方法
原理分析完了，说说解决方法吧。对于这样的指定IP的数据包拦截，解决方法无非两种：代理和反向代理。

事实上，代理并不太可行。原因如下：
1. 使用代理，我们所有流量都会经过代理程序或代理服务器，一来延迟可能会增大，二来安全性未必高
1. 代理服务器容易被端口扫描攻击
1. 使用海外代理服务器有可能违法

因此我选择使用反向代理模式。最开始我尝试在Cloudflare的CDN服务器上使用JSProxy，但是后来发现这套源码并不能直接访问到内容，而是通过Javascript异步加载内容。其它的一些Web反向代理程序也是这样。

后来我发现nginx自身就带有反向代理功能，尤其是加上AppNode、宝塔等Web服务器面板后配置十分简单。我们只需要建立一个反向代理网站并且配置传递参数和目标服务器即可。AppNode更加方便，可以直接设置访问的域名为目标服务器传递参数。

一开始我使用FoxReplace浏览器插件对所有文档进行URL替换，但是发现部分存储在Javascript脚本或者异步获取的文档中的URL无法被正常替换。于是我使用URLRedirector插件尝试进行URL替换，但是发现这有一个很大的问题：Github源站有很多个子域名，如果我要替换的话，我需要解析很多个域名来一一对应，这不实际。

最后我找到了很好的方法：在反向代理服务器上配置`*.githubusercontent.com`即所有源站域名，然后在本地hosts文件中将这些域名解析到我的反向代理服务器。

注意还有一个就是刚开始访问也可能不成功，我需要使用开发者工具——网络选项查看并且找出提示“证书不匹配”的链接打开，选择“添加例外”即可。

参考链接：
1. https://www.ipaddress.com

  [1]: https://www.ipaddress.com
