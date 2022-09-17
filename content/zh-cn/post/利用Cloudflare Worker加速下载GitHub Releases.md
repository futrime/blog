---
author: Futrime
title: 利用Cloudflare Worker加速下载GitHub Releases
description: GitHub Releases在国内访问很慢，Gitee的镜像也没有涵盖GitHub Releases，但是可以依靠Cloudflare Worker加速到10Mbps.
date: 2020-11-02T21:49:46+08:00
categories:
    - 奇技淫巧
    - 经验杂谈
tags:
    - Cloudflare
    - 下载
    - GitHub
    - Cloudflare Worker
    - 加速
    - GitHub Releases
image: https://i.loli.net/2020/11/02/RLsICHvnuewVaQK.png
draft: false
---

今年4月份我写了篇文章[利用Cloudflare Workers加速下载海外服务器的文件](../利用cloudflare-workers加速下载海外服务器的文件/) ，讲了下加速下载海外服务器文件的方法。但是最近在GitHub下载东西的时候发现GitHub Releases并不能获取到直链，而是会出现跳转，这样一来，原来的方法就失效了。

但是对于跳转也并非没有解决方案，其实本质就是把跳转的Header中的`location`改为经过代理的地址，代码如下：

```javascript
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

/**
 * Fetch and log a given request object
 * @param {Request} request
 */
async function handleRequest(request) {
  const url = request.url.substring(
      new URL(request.url).origin.length + 1
    );
  let new_request = new Request(
    url,
    {
      headers: request.headers,
      cf: request.cf,
      method: request.method,
      body: request.body,
      redirect: request.redirect
    }
  );
  let response = await fetch(new_request);
  if(response.status == 301 || response.status == 302){
    response = new Response(response.body, response)
    response.headers.set(
      "location",
      (new URL(request.url).origin) +
        '/' + response.headers.get('location')
      )
  }
  return response;
}
```

中间也折腾了蛮久，主要问题是`fetch()`获取的response是附带const属性的，不能直接修改，因此需要新建一个。
