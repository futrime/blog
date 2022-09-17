---
author: Futrime
title: Python Web Server from Scratch
description: Sometimes we'd like to copy some file from our computer to our mobile phone and other devices at home. However, common chat applications have set a limit of max upload size or has limited the upload and download speed. Then Python has a place.
date: 2020-10-09T08:46:51Z
image: https://i.loli.net/2020/10/09/X8pBQPr2KmwkiTt.jpg
categories:
    - Experience
    - Skills
tags:
    - Python
    - Web
    - Server
draft: false
---

First we need to install Python 3.x, if using Linux please refer to [Build Python Environment in Linux][2]. If using Windows, you can download from official straightly: https://www.python.org/downloads/windows/, The .zip file is portable version, other versions need installation.

![Python Distributions](https://i.loli.net/2020/10/09/jxvBQd2PMDgkqnI.png)

（Windows Users）Go to the folder to share, right click with `SHIFT`, choose `Open Powershell in this folder` or `Open command line in this folder`.

Type command `python3 -m http.server` （`python3` might be replaced by `python` or `py`）, it uses port 8000 for default.

![Demo](https://i.loli.net/2020/10/09/k1bzDueGnf4YArh.png)

In order to use a specific port, type `python3 -m http.server <PORT>`. To designate the folder, type `python3 -m http.server -d <FOLDER NAME>`.

There is some more things to do with Portable version, you have to type the full path of Python executable program instead of the abbreviations like `python`.

To make a .py script, create a new file and type content below,

```
import http.server
import socketserver

PORT = 8000 # Port

Handler = http.server.SimpleHTTPRequestHandler

with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print("serving at port", PORT)
    httpd.serve_forever()
```

References：
1. https://docs.python.org/3.7/library/http.server.html
1. https://www.python.org/downloads/windows/
