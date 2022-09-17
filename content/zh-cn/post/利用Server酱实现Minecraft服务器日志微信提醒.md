---
title: 利用Server酱实现Minecraft服务器日志微信提醒
date: 2020-05-17 11:15:00
draft: false
author: Futrime
description: 在Minecraft服务器日常使用中，我们常常想要知道服务器上发生了什么，有谁进来了，有谁退出去了，或者是服务器自己是不是出了错误。在这篇博客中我将会简要介绍如何利用Server酱实现Minecraft服务器日志微信提醒。
image: https://futrime.gitee.io/image-cdn/2020/04/4099228983.jpg
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Minecraft
  - 服务器
  - Server酱
  - 日志
  - 微信
---

## API申请

首先我们需要前往Server酱官网注册账号以获得自己的SCKEY：

![Screenshot_2020-04-22 首页 Server酱.png][3]

可以直接使用Github登录，十分方便。登陆后选择`微信推送`，点击绑定。用自己的微信扫描二维码，关注公众号`方糖`，然后在扫描一次即可绑定，注意要回到网页点击`检查结果并确认绑定`。

然后切换到`发送消息`栏，复制自己的SCKEY。

## Docker服务器配置

对于使用Docker搭建服务器的用户来说，配置十分方便且不需要暂停服务器。

首先我们使用`docker ps`指令查找到Minecraft服务端容器名称,如图为`mcserver`：

![Screenshot_2020-04-22 首页 Server酱.png][4]

然后新建一个Python脚本（没有Python环境？参考：[Linux下搭建Python环境][5]），输入以下内容并将对应内容改为自己的信息（去掉<>）：

```
import threading
import time
import urllib.request
import urllib.parse
import os

last_msg = ""
def time_handler():
    msg = os.popen("docker logs --tail 1 <服务端容器名称>")
    msg = msg.read()
    print(msg[:-1])
    global last_msg
    if msg[-26:] != "Running AutoCompaction...\n" and msg != last_msg:
        urllib.request.urlopen("https://sc.ftqq.com/<你的SCKEY>.send?text=" + urllib.parse.quote(msg[27:-25])) # 其中 msg[27:-25] 是特别为玩家上线/下线提醒设置的，如果希望看到完整信息，请改为 msg
    else:
        print("Nothing to do")
    last_msg = msg
    global timer
    timer = threading.Timer(5, time_handler)
    timer.start()

time_handler()
```

然后在终端中输入`nohup python3 <你的脚本名称>`并运行即可。

## 全局服务器配置

如果你使用nohup运行服务器，那么直接使用以下代码即可：

```
import threading
import time
import urllib.request
import urllib.parse
import os

last_msg = ""
def time_handler():
    msg = os.popen("tail -n 1 nohup.out") # 如果你自定义了nohup输出地址，请修改
    msg = msg.read()
    print(msg[:-1])
    global last_msg
    if msg[-26:] != "Running AutoCompaction...\n" and msg != last_msg:
        urllib.request.urlopen("https://sc.ftqq.com/<你的SCKEY>.send?text=" + urllib.parse.quote(msg[27:-25])) # 其中 msg[27:-25] 是特别为玩家上线/下线提醒设置的，如果希望看到完整信息，请改为 msg
    else:
        print("Nothing to do")
    last_msg = msg
    global timer
    timer = threading.Timer(5, time_handler)
    timer.start()

time_handler()
```

如果你使用screen运行服务端，那么你需要先结束原来的服务端并用`exit`命令退出screen环境。然后运行`screen -L -dmS mc`建立自动日志screen环境。然后通过`screen -r mc`进入screen环境中手动启动服务器。

然后按`Ctrl+A,D`离开screen环境，用`ls`命令可以看到当前目录中出现了`screenlog.0`文件（也可能是`screenlog.1`如此类推）,新建Python脚本（没有Python环境？参考：[Linux下搭建Python环境][5]），输入：

```
import threading
import time
import urllib.request
import urllib.parse

last_msg = ""
def time_handler():
    msg = open("screenlog.0") # 或者screenlog.1以此类推
    msg = msg.readlines()[-1]
    print(msg[:-1])
    global last_msg
    if msg[-26:] != "Running AutoCompaction...\n" and msg != last_msg:
        urllib.request.urlopen("https://sc.ftqq.com/<你的SCKEY>.send?text=" + urllib.parse.quote("[Sunny s World] " + msg[27:-25]))
    else:
        print("Nothing to do.")
    last_msg = msg
    global timer
    timer = threading.Timer(1, time_handler)
    timer.start()

time_handler()
```

然后在终端中输入`nohup python3 <你的脚本名称>`即可。

参考链接：
1. https://sc.ftqq.com/
2. https://blog.futrime.com/hacking/302.html
3. https://blog.futrime.com/hacking/131.html
4. https://docs.docker.com/

  [3]: https://futrime.gitee.io/image-cdn/2020/04/2373489148.png
  [4]: https://futrime.gitee.io/image-cdn/2020/04/4180118812.png
  [5]: https://blog.futrime.com/hacking/131.html
