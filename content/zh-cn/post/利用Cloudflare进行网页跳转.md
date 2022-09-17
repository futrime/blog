---
title: 利用Cloudflare进行网页跳转
date: 2020-05-16 05:16:00
draft: false
author: Futrime
description: 有时候我们注册了多个域名，或者一个域名有多个子域名需要应用到同一个网站上，但是我们又不希望过多域名导致搜索引擎权重分散，那么我们就可以通过Cloudflare进行网页跳转。在这篇文章中我将会简单讲讲如何使用Cloudflare进行网页跳转。
image: https://futrime.gitee.io/image-cdn/2020/04/641371271.jpg
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Cloudflare
  - 域名
  - 跳转
---

## 绑定域名

首先我们需要注册一个Cloudflare账号。

拥有Cloudflare账号后，点击右上角的`添加站点`，输入自己的主域名（注意应当是`example.com`这样的形式，如果输入了`www.example.com`的话只能跳转`xxx.www.example.com`这样的域名了。

选择第一个Free计划：

![批注 2020-04-21 155253.jpg][2]

等待自动扫描添加现有DNS记录，不过往往都会无法识别所有记录，所以需要手动添加，注意此时需要跳转的域名就必须要勾选`流量经过Cloudflare`，而不需要跳转的域名可以不勾选（也就是把橙色云点成灰色云）（本质上就是CDN，但是如果你的服务器本来在国内访问就很快的话建议不勾选）。另外一点，建议需要跳转的域名使用A记录解析到1.1.1.1：

![批注 2020-04-21 155453.jpg][3]

然后按照提示修改现有NS服务器，这个需要前往域名注册商修改：

![批注 2020-04-21 155733.jpg][4]

随后点击确认，在新的页面中点击`稍后设置`。

## 设置跳转

在域名管理界面中选择`Workers`，然后选择`管理Workers`，然后点击`创建Worker`,进入Worker编辑界面:

![批注 2020-04-21 160158.jpg][5]

在`脚本`栏中输入如下内容：

```
async function handleRequest(request) {
  return Response.redirect(someURLToRedirectTo, code)
}
addEventListener('fetch', async event => {
  event.respondWith(handleRequest(event.request))
})
/**
 * Example Input
 * @param {Request} url where to redirect the response
 * @param {number?=301|302} type permanent or temporary redirect
 */
const someURLToRedirectTo = '<要跳转到的网址>'
const code = <跳转代码，可以是永久迁移(301)或者临时迁移(302)，建议301>
```

然后点击`HTTP`栏中的`发送`，如果右边显示重定向信息，那么就可以了：

![批注 2020-04-21 160454.jpg][6]

随后在`脚本`栏上方的域名中修改自定义子域名，比如`redirect.futrime.workers.dev`（不修改也行）。

然后回到域名管理界面的`Workers`栏，点击`添加路由`，输入被跳转的网址并选择刚才的Worker:

![批注 2020-04-21 160732.jpg][7]

这个过程可以重复多次，想要跳转多少就跳转多少。

然后就可以尝试访问自己的网站了看效果了。

参考链接：
1. https://www.cloudflare.com
2. https://www.workers.dev

  [2]: https://futrime.gitee.io/image-cdn/2020/04/424338012.jpg
  [3]: https://futrime.gitee.io/image-cdn/2020/04/1745231638.jpg
  [4]: https://futrime.gitee.io/image-cdn/2020/04/3424933613.jpg
  [5]: https://futrime.gitee.io/image-cdn/2020/04/2408418169.jpg
  [6]: https://futrime.gitee.io/image-cdn/2020/04/2049048597.jpg
  [7]: https://futrime.gitee.io/image-cdn/2020/04/1932412691.jpg
