---
title: 谈谈我们现在的HTTP环境
date: 2020-02-17 00:00:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/02/3662435233.jpg
description: 前段时间我发了一篇文章《谈谈我们现在的DNS环境》，其实里面遇到的很多问题在HTTP协议中也遇到了，那么我就顺带着讲一下吧。
categories:
  - 经验杂谈
tags:
  - 网络
  - 环境
  - Web
  - 运营商
  - HTTP
  - HTTP协议
  - 劫持
  - HTTP劫持
  - 广告
  - 弹窗
---

作为Web世界最基础、最重要的协议之一，DNS协议承担了我们平时见到的所有网站的解析任务。但是随着社会和网络技术的发展，DNS协议在技术上逐渐落伍了，许许多多的漏洞和缺陷逐渐暴露了出来，并且被一些组织和机构所利用。

### HTTP原理
HTTP的原理其实没有什么好说的，具体可以参考[百度百科][3]。不过需要注意的一点就是HTTP版本：最早广泛使用的版本是HTTP/0.9，现在已经全面弃用。后来发展出了成熟的HTTP/1.0，目前仍然有少量旧网站使用。目前来说最广泛使用的是HTTP/1.1标准，也就是我们最常见的HTTP协议内容。前几年HTTP/2标准诞生，成为当下最安全最可靠的标准。但是这个标准必须要搭配HTTPS协议使用，因此这篇文章不讨论。

### HTTP技术存在的问题
和上篇博文中的DNS技术存在的问题类似，HTTP的问题也是来源于它并没有提供加密。也就是说，当我们访问HTTP网站的时候，数据包经过的所有路由器和服务器是可以跟踪记录、拦截吞噬甚至篡改它的。这就相当于我们去寄信，结果信封忘记封了，那么快递员就可以神不知鬼不觉（我不知服务器不觉）的情况下窃取甚至篡改里面的文字。这种行为我们称之为*HTTP劫持*。

很多人以为自己难以遇到HTTP劫持，但其实只不过是因为我们司空见惯罢了。HTTP劫持大多数情况下被宽带服务商用于在网页中添加广告，但是由于在中国，很多用户的计算机早就被各种各样的违法或合法甚至官方推广软件弹出的广告训练得“百毒不侵”了，而对于HTTP劫持的广告视而不见。

![2.jpg][4]

上面这幅图右下角的悬浮广告是不是很熟悉？如果我们正在浏览一个网页，这个网页以前没有广告或者其他部分没有广告，却在某一天某一次访问的时候出现了这样的广告，那就看看地址栏吧。如果没有`https://`字样，或者没有小锁标志（没有使用安全的HTTPS协议），那九成是被劫持了。我们也可以右键点击这个广告，查看它的网址，如果这个网址是类似`gd.163.cn/.....`这样的话那肯定是劫持了。

有时候劫持并不是悬浮广告，而是以干扰性更强的弹窗广告呈现。如下图所示：

![2020-02-12-114136_1440x900_scrot.png][5]

我使用了Firefox的弹窗拦截功能，所以没有直接弹出来。

甚至还有更有干扰性的手段：在整个网页上增加一个覆盖层，只要点击了网页任何一个部分就会跳转到广告页面，如下：

![2020-02-12-114150_1440x900_scrot.png][6]

不过广告算小事，万一弹出来的是钓鱼网站（常常在公共Wi-Fi上容易出现）就麻烦大了。

### 解决办法
其实很简单，只要我们确保访问的网站是支持HTTPS协议的就可以了。多看看地址栏，确保有`https://`前缀。另外我们也可以使用[HTTPS Everywhere][7]、[Smart HTTPS][8]这类插件来帮助我们实现安全访问。

不过令人头痛的是，目前国内还有很多知名网站不支持HTTPS。（点名：[中关村在线][9]，不过居然Firefox官方论坛也不支持HTTPS：[火狐社区][10]）希望大家尽快重视起来吧。

如果你是站长（像我一样），记得启用**HTTP强制传输安全服务**（HTTP Strict Transport Security，HSTS）哦。请参考：https://baike.baidu.com/item/HSTS/8665782


----------

后来我发现原来 http://mozilla.com.cn 根本就不是Mozilla Firefox官方的社区，完全就是冒牌货。Mozilla Firefox社区请认准： https://www.mozilla.org 。

参考链接：
1. https://baike.baidu.com/item/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E9%80%81%E5%8D%8F%E8%AE%AE
1. https://blog.futrime.com/freetalk/186.html
1. https://addons.mozilla.org/zh-CN/firefox/addon/https-everywhere/
1. https://addons.mozilla.org/zh-CN/firefox/addon/smart-https-revived/
1. https://developer.mozilla.org
1. https://baike.baidu.com/item/HSTS/8665782


  [1]: https://blog.futrime.com/freetalk/186.html
  [3]: https://baike.baidu.com/item/%E8%B6%85%E6%96%87%E6%9C%AC%E4%BC%A0%E9%80%81%E5%8D%8F%E8%AE%AE
  [4]: https://futrime.gitee.io/image-cdn/2020/02/3121389157.jpg
  [5]: https://futrime.gitee.io/image-cdn/2020/02/3558982608.png
  [6]: https://futrime.gitee.io/image-cdn/2020/02/1434072741.png
  [7]: https://addons.mozilla.org/zh-CN/firefox/addon/https-everywhere/
  [8]: https://addons.mozilla.org/zh-CN/firefox/addon/smart-https-revived/
  [9]: http://www.zol.com.cn/
  [10]: http://mozilla.com.cn
