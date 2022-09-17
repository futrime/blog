---
title: 利用Cloudflare Workers加速下载海外服务器的文件
date: 2020-04-29 15:00:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/04/199322626.png
description: 有时候我们需要下载某个海外服务器上的大文件，但是这个服务器到我们这里的网络质量很差，经常会断流或者速度很慢，此时我们就可以使用Cloudflare Workers为我们加速下载。
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Cloudflare
  - 下载
  - 海外
  - 加速
  - Cloudflare Workers
---

首先打开[Cloudflare Workers][2],如果没有账号则需要注册账号。登陆进去后，点击 创建Worker。在左上角输入自定义域名前缀，如图：

![批注 2020-04-29 145534.jpg][3]

然后在左栏输入以下代码：

```javascript
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

/**
 * Fetch and log a given request object
 * @param {Request} request
 */
async function handleRequest(request) {
  const response = await fetch(request.url.substring(<你的Worker网址长度>));
  console.log(request)
  return response
}
```

其中Worker网址长度也就是你自定义的网址`https://xxx.xxx.workers.dev`这个字符串的长度，可以打开记事本，输入这个网址（最后不要多加“/”），然后看到右下角“第n列”，这个n就是你的Worker网址长度。

![批注 2020-04-29 145750.jpg][4]

点击保存并部署即可，以后要下载大文件的时候可以直接打开`https://xxx.xxx.workers.dev/<大文件地址>`。

参考链接：
1. https://workers.cloudflare.com/
2. https://developers.cloudflare.com/workers/templates
3. https://developers.cloudflare.com/workers/reference/apis/fetch/

  [2]: https://workers.cloudflare.com/
  [3]: https://futrime.gitee.io/image-cdn/2020/04/1880586063.jpg
  [4]: https://futrime.gitee.io/image-cdn/2020/04/499250773.jpg
