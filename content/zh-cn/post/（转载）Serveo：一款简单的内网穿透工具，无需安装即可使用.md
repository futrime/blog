---
title: Serveo：一款简单的内网穿透工具，无需安装即可使用
date: 2019-08-10
draft: false
author: Futrime
description: Serveo是一个SSH服务器，仅用于远程端口转发，可以快速将本地端口暴露在外网。官方声称其为Ngrok的绝佳替代品，对其优点是使用现有的SSH客户端，无需安装客户端即可完成端口转发。当用户连接到Serveo时，他们会获得一个公共URL，任何人都可以使用它来连接到他们的localhost服务器。
image: https://futrime.gitee.io/image-cdn/2019/08/2287437762.jpg
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - 转载
  - 内网穿透
  - SSH
---

服务器是国外的，国内延迟有点高

官方地址：[serveo.net][2]

使用要求：可以使用SSH，并且能连接到互联网，Linux、Windows等系统都行。

1、转发HTTP
将本地3000端口穿透到公网中，使用命令：

#要转发其它端口的自行替换

    ssh -R 80:localhost:3000 serveo.net

第一次如果有提示，选择yes即可，之后会为你随机生成一个serveo.net二级域名，然后就可以使用浏览器间接访问本地的localhost:3000了。

如果要指定二级域名，可以使用命令：

#这里默认为sunny.serveo.net，自行替换即可

    ssh -R sunny:80:localhost:3000 serveo.net

此时你就可以在外网使用sunny.serveo.net访问你本地的localhost:3000了。

2、转发SSH
将本地22端口穿透到公网中，使用命令：

#可以自行设置名称，这里默认rats

    ssh -R sunny:22:localhost:22 serveo.net

接下来就可以登录该内网服务器了，使用命令：

    ssh -J serveo.net root@sunny

3、转发TCP
将本地1492端口穿透到公网中，使用命令：

#可以自行设置公网端口，这里默认1492

    ssh -R 1492:localhost:1492 serveo.net

进程守护
这里官方推荐使用AutoSSH，作用是一旦SSH连接超时或停止传递流量，则根据需要重新启动它。

1、安装AutoSSH

    #Debian/Ubuntu系统
    apt install autossh -y

    #CentOS系统
    yum install autossh -y

2、使用Systemd

    只适用于CentOS 7、Debian 8+、Ubuntu 16+等。

    #输入你的转发命令，去掉开头的ssh即可
    serveo="-R 80:localhost:3000 serveo.net"
    #将以下代码一起复制到SSH运行
    cat > /etc/systemd/system/autossh.service <<EOF
    [Unit]
    Description=autossh
    After=network.target

    [Service]
    Type=simple
    Environment="AUTOSSH_GATETIME=0"
    ExecStart=$(command -v autossh) -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" $serveo
    Restart=on-abort

    [Install]
    WantedBy=multi-user.target
    EOF
    开始启动并设置开机自启：

    systemctl start autossh
    systemctl enable autossh

最后更多的命令和使用可以直接查看官方文档→[传送门][3]。


转自[moerats][4]

  [2]: http://serveo.net
  [3]: http://serveo.net/
  [4]: https://www.moerats.com/archives/990/
