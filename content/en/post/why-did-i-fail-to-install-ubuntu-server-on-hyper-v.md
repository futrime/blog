---
title: Why Did I Fail to Install Ubuntu Server on Hyper-V?
description: It keeps failing...
author: Futrime

categories:
    - Experience
date: 2023-10-01T11:28:00+08:00
draft: false
tags:
    - Hyper-V
    - Ubuntu
    - LVM
---

When installing Ubuntu 20.04.6 LTS on Hyper-V, I keep encountering fatal errors without clear messages. I tried to install it on a physical machine and it worked. So I guess it's the problem of Hyper-V. After some research, I found that it's the problem of LVM.

A workaround is to disable LVM during installation.
