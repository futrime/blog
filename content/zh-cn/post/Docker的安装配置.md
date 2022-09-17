---
title: Docker的安装配置
image: https://futrime.gitee.io/image-cdn/2020/04/2376635441.png
date: 2020-05-07 05:21:00
draft: false
author: Futrime
description: 作为容器虚拟化技术的翘楚，Docker颇受各行各业的人士喜爱。一方面它可以省去各个环境之间互相污染的麻烦，又可以得到比虚拟机高得多的效率，两全其美。在这篇博客中我将简单介绍Docker的安装配置、基本使用方法以及当Docker容器很多的时候非常重要的一步：资源分配。
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Docker
  - 安装
---

要学会Docker，最简单的方法就是看文档：[Docker Documentation][2] ，但是全英文的文档对于中文环境下的用户并不友好，而当前很多国内的教程都旧了，不能跟上Docker版本迭代的步伐。在这里我姑且充当翻译，把文档和自己的理解略作结合作个总结。

## 安装配置
### Windows/Mac
在Windows或Mac下安装Docker十分方便，直接前往 [Get Started with Docker][3] 下载Docker Desktop，按照提示步骤安装即可。

### Ubuntu
首先卸载旧版本（如果有）：
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
然后安装一些必要前置组件：
```
sudo apt-get update && sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
添加Docker CE下载仓库（使用官方源）：
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
上面的`[arch=amd64]`应根据自己的平台选择，可为：32位ARM`armhf`，64位ARM`arm64`，IBM Power`ppc64el`，IBM Z`s390x`。

官方源在中国大陆访问质量可能不佳，此时可以更换为阿里云镜像源等，此处以阿里云镜像源为例。

添加Docker CE下载仓库（使用阿里云镜像源）：
```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```
上面的`[arch=amd64]`应根据自己的平台选择，可为：32位ARM`armhf`，64位ARM`arm64`，IBM Power`ppc64el`，IBM Z`s390x`。

然后就可以安装Docker CE啦：
```
sudo apt-get update -y
sudo apt-get install docker-ce -y
```

按照Docker官方文档，还需要安装`docker-ce-cli`和`containered.io`，但是阿里云镜像源文档没有提示，可能是新版Docker特性（存疑）：
```
sudo apt-get install docker-ce-cli containerd.io -y
```

### CentOS
Docker在CentOS上的安装似乎比Ubuntu方便许多。

首先卸载旧版本（如果有）：
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

安装前置组件：
```
sudo yum install -y yum-utils
```

阿里云镜像源文档还给了更多前置组件（此处存疑）：
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

添加Docker CE下载仓库（使用官方源）：
```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

官方源在中国大陆访问质量可能不佳，此时可以更换为阿里云镜像源等，此处以阿里云镜像源为例。

添加Docker CE下载仓库（使用阿里云镜像源）：
```
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

然后就可以安装Docker CE啦：
```
sudo yum install docker-ce
```

按照Docker官方文档，还需要安装`docker-ce-cli`和`containered.io`，但是阿里云镜像源文档没有提示，可能是新版Docker特性（存疑）：
```
sudo yum install docker-ce-cli containerd.io
```

### Debian
Debian和Ubuntu很相似，似乎只有软件源不一样，毕竟本是同根生。

首先卸载旧版本（如果有）：
```
sudo apt-get remove docker docker-engine docker.io containerd runc
```
然后安装一些必要前置组件：
```
sudo apt-get update && sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
添加Docker CE下载仓库（使用官方源）：
```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
```
上面的`[arch=amd64]`应根据自己的平台选择，可为：32位ARM`armhf`，64位ARM`arm64`，IBM Power`ppc64el`，IBM Z`s390x`。

官方源在中国大陆访问质量可能不佳，此时可以更换为阿里云镜像源等，此处以阿里云镜像源为例。

添加Docker CE下载仓库（使用阿里云镜像源）：
```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/debian $(lsb_release -cs) stable"
```
上面的`[arch=amd64]`应根据自己的平台选择，可为：32位ARM`armhf`，64位ARM`arm64`，IBM Power`ppc64el`，IBM Z`s390x`。

然后就可以安装Docker CE啦：
```
sudo apt-get update -y
sudo apt-get install docker-ce -y
```

按照Docker官方文档，还需要安装`docker-ce-cli`和`containered.io`，但是阿里云镜像源文档没有提示，可能是新版Docker特性（存疑）：
```
sudo apt-get install docker-ce-cli containerd.io -y
```

## 使用Docker Hub镜像
在中国大陆访问Docker Hub官方源的连接质量十分堪忧，甚至快到完全不能用的地步，此时使用Docker Hub镜像显得尤为重要。以下以腾讯云Docker Hub加速器为例。

### Mac
我没有Mac，没办法详细演示，但是和Windows基本一致（都是Docker Desktop），请参考Windows部分。

### Windows
先运行Docker Desktop，然后右击右下角托盘栏中的Docker Desktop图标，点击Settings：

![批注 2020-04-26 181034.jpg][4]

切换到Docker Engine选项卡,在右栏的`"registry-mirrors": []`中输入`"https://mirror.ccs.tencentyun.com"`,如图：

![批注 2020-04-26 181320.jpg][5]

点击 Apply & Restart 然后静待Docker Desktop重启即可（要几分钟）。

参考链接：
1. https://docs.docker.com/
2. https://www.docker.com/get-started
3. https://developer.aliyun.com/mirror/docker-ce
4. https://cloud.tencent.com/document/product/457/9113

  [2]: https://docs.docker.com/
  [3]: https://www.docker.com/get-started
  [4]: https://futrime.gitee.io/image-cdn/2020/04/800717711.jpg
  [5]: https://futrime.gitee.io/image-cdn/2020/04/2814306674.jpg
