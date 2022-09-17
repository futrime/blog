---
author: Futrime
title: Open Browser in Python
description: We can use webbrowser module to open browser in Python.
date: 2020-10-09T08:19:31Z
image: https://i.loli.net/2020/10/09/3uBZifRweXqHLx9.jpg
categories:
    - Experience
    - Skills
tags:
    - Python
    - Browser
draft: false
---

Command line：

    python -m webbrowser -t "<URL>"

.py script:

```python
url = '<URL>'

# Open straightly (default)
webbrowser.open(url)

# Open in new tab
webbrowser.open_new_tab(url)

# Open in new window
webbrowser.open_new(url)

# Assign a browser（open_new、open_new_tab support this way too）
webbrowser.get('<Browser Code>').open(url)
```

The browser codes is among `mozilla` `firefox` （Mozilla Firefox） `opera` (Opera) `windows-default` (Windows default browser） `macosx` （Mac OS default browser） `safari` （Safari）`google-chrome` `chrome` （Chrome） `chromium` `chromium-browser` （Chromium and other browsers based on chromium）

References：
1. https://docs.python.org/3.7/library/webbrowser.html
