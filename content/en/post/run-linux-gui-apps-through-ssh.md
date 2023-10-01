---
title: Run Linux GUI Apps through SSH
description: For Linux server distributions without GUI...
author: Futrime

categories:
    - Experience
date: 2023-10-01T11:28:00+08:00
draft: false
tags:
    - Linux
    - SSH
    - GUI
    - X11
---

As a Linux server and Windows user, I often need to run Linux GUI apps but do not wanna install a GUI on the server. So I use SSH to run GUI apps on the server and display them on my Windows machine.

## Install X11 Server on Windows

Download and install VcXsrv from <https://sourceforge.net/projects/vcxsrv/>.

Run the XLauncher and keep the default settings.

## Install X11 Client on Linux

```bash
sudo apt-get install xorg openbox
```

## Enable X11 forwarding on the server

Edit `/etc/ssh/sshd_config` and modify the following line:

```bash
X11Forwarding yes
```

## Restart SSH service

```bash
sudo service ssh restart
```

## Connect to the server

```bash
ssh -X user@server-ip-address
```

## Run GUI apps

Run xeyes to test:

```bash
xeyes
```
