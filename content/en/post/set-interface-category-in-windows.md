---
title: Set Interface Category in Windows
description: If a proxy is required by programs in Hyper-V virtual machines...
author: Futrime

categories:
    - Experience
date: 2023-09-10T11:28:00+08:00
draft: false
tags:
    - Network
    - Virtual Machine
    - Proxy
---

Sometimes, some programs in a Hyper-V virtual machine need to access some network resources outside firewalls. Therefore, a proxy is required. However, by no means should we set the proxy in the virtual machine, taking into consideration the fact that setting up proxy in the virtual machine is a waste of time and resources. Instead, we can set the proxy in the host machine, and then share the proxy to the virtual machine.

1. Before we start, we need to make sure that the proxy program is available for private network. 

2. Get network interfaces with `Get-NetConnectionProfile`.

   ```powershell
   Get-NetConnectionProfile
   ```

3. Set the interface category to private with `Set-NetConnectionProfile`.

   ```powershell
   Set-NetConnectionProfile -InterfaceIndex <InterfaceIndex> -NetworkCategory Private
   ```
