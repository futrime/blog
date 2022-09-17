---
title: 什么是HTTPS降级攻击？遇到了怎么办？
date: 2020-02-23 00:00:00
draft: false
author: Futrime
description: HTTPS协议降级攻击可以说是源于HTTPS协议对于当下HTTP协议主导的世界的一个妥协。这篇文章我就简单谈谈HTTPS降级攻击的原理和预防方法。
image: https://futrime.gitee.io/image-cdn/2020/02/3893297248.png
categories:
  - 经验杂谈
tags:
  - 安全
  - 网络安全
  - 信息安全
  - HTTPS降级攻击
  - HTTPS
  - 网络攻击
  - SSL
  - TLS
  - HSTS
---

前段时间我发布了文章[《谈谈我们现在的HTTP环境》][2]，当时我认为只要使用了HTTPS就可以很好地保证上网的安全了,实————————直到我看到了这篇文章：[《HTTPS协议降级攻击原理》][3]。我才知道一句真理：道高一尺，魔高一丈\手动狗头。结果没过多久，又看到了这篇文章：[《躲避HSTS的HTTPS劫持》][4]，我才知道，原来魔不是高一丈，而是高了一里\滑稽\当然这里不讲。

既然知道了，那就理所应当地承担起科普的责任~~水博客的方法~~给大家讲一下原理和预防方法吧。

### 基本原理
在HTTPS协议设计之初，并不是使用先进的TLS协议，而是使用SSL协议或者旧版的TLS协议。这些协议中自身存在一些漏洞可以很容易被攻击（即使是所谓“新型”的SSL 3.0和TLS 1.1）。虽然SSL协议现在已经弃用了，但是为了兼容性（又是这个该死的兼容性）考虑，我们的浏览器和Web服务器往往都会兼容性支持旧的不安全的协议。（顺带一提，即使是当前广泛使用的TLS 1.2也有类似漏洞，新站长们记得升级TLS 1.3）

浏览器和Web服务器间是通过ClientHello包和ServerHello包互相确认兼容的协议，并挑选最新那个出来用来传输数据。而这两个Hello包为了兼容性考虑（又是它！），一般都会使用旧的保守的有漏洞的协议。假如攻击者捕获了这两个Hello包，就可以利用已知的漏洞篡改内容，使得服务器和浏览器都以为对方只能使用旧的协议，从而在整个时长100个请求的连接中采用有漏洞的协议。进而攻击者就可以很容易窃听了。

具体方式请看： https://zhuanlan.zhihu.com/p/22917510

### 预防方法
很简单，直接在浏览器设置中禁用SSL协议的支持就好了。\狗头

站长们也不要忘记了拒绝SSL协议的访问哦。

参考链接：
1. https://www.cloudflare.com/learning/ssl/what-is-https/
1. https://blog.futrime.com/freetalk/190.html
1. https://zhuanlan.zhihu.com/p/22917510
1. https://www.cnblogs.com/index-html/p/https_hijack_hsts.html
1. https://baike.baidu.com/item/ssl

  [2]: https://blog.futrime.com/freetalk/190.html
  [3]: https://zhuanlan.zhihu.com/p/22917510
  [4]: https://www.cnblogs.com/index-html/p/https_hijack_hsts.html
