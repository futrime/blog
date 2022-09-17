---
author: Futrime
title: Python实现打开浏览器
description: 有时候我们的Python演示脚本需要使用浏览器访问，此时可以用webbrowser库实现打开浏览器。
date: 2020-07-06T07:45:42+08:00
categories:
    - 经验杂谈
    - 奇技淫巧
tags:
    - Python
    - 浏览器
image: https://i.loli.net/2020/10/31/14aSitNEOqmdKL3.jpg
draft: false
---

命令行用法：

    python -m webbrowser -t "<网址>"

脚本用法，需要建立Python文件：

```python
url = '<网址>'

# 直接打开（系统默认方式）
webbrowser.open(url)

# 新标签页打开
webbrowser.open_new_tab(url)

# 新窗口打开
webbrowser.open_new(url)

# 指定浏览器打开（也可以用open_new、open_new_tab）
webbrowser.get('<浏览器代号>').open(url)
```

常见浏览器代号包括 `mozilla` `firefox` （Mozilla Firefox） `opera` (Opera) `windows-default` (Windows默认浏览器） `macosx` （Mac OS默认浏览器） `safari` （Safari）`google-chrome` `chrome` （Chrome） `chromium` `chromium-browser` （Chromium，常见国产浏览器）

参考链接：
1. https://docs.python.org/zh-cn/3.7/library/webbrowser.html?highlight=webbrowser
