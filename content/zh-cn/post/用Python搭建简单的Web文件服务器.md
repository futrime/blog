---
author: Futrime
title: 用Python搭建简单的Web文件服务器
description: 有时我们想要把电脑上的文件传到手机或其它设备上，又苦于微信/QQ/百度网盘等软件文件大小限制或极慢的速度，此时Python Web服务器便十分重要。
date: 2020-07-07T07:45:42+08:00
categories:
    - 经验杂谈
    - 奇技淫巧
tags:
    - Python
    - Web
    - 服务器
image: https://i.loli.net/2020/10/31/UF6BvbiH2ECuTML.jpg
draft: false
---

首先要装Python 3，Linux用户请参考[在Unix平台中使用Python](https://docs.python.org/zh-cn/3/using/unix.html)，Windows用户可以直接在官网下载安装：https://www.python.org/downloads/windows/ ，.zip文件是Portable版本，可以直接运行，其它的需要安装。

![Screenshot_2020-07-08 Python Releases for Windows.png][3]

（Windows用户）然后去到需要分享的文件夹，按住 `Shift` 并右键单击空白处，选择 `在此处打开Powershell窗口` 或者 `在此处打开命令提示符`。

![批注 2020-07-08 144550.jpg][4]

输入命令 `python3 -m http.server` （Windows下`python3`可能要换成`python`或`py`）即可，默认使用端口8000.

![Screenshot_2020-07-08 Directory listing for .png][5]

如果需要指定端口，输入  `python3 -m http.server <端口号>`，需要指定目录，输入  `python3 -m http.server -d <目录>`。

Portable版本比较麻烦，需要输入完整Python程序路径而非`python`这样的简写。

如果需要做成可以直接用的Python脚本，新建Python文件，输入以下内容：

```python
import http.server
import socketserver

PORT = 8000 # 端口号

Handler = http.server.SimpleHTTPRequestHandler

with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print("serving at port", PORT)
    httpd.serve_forever()
```

参考链接：
1. https://docs.python.org/zh-cn/3.7/library/http.server.html
1. https://www.python.org/downloads/windows/

  [3]: https://i.loli.net/2020/10/31/1QDT8heP7xC4mqS.png
  [4]: https://i.loli.net/2020/10/31/ahmk4ZXgqyPRr59.jpg
  [5]: https://i.loli.net/2020/10/31/xWmPVpLiNh4v3au.png
