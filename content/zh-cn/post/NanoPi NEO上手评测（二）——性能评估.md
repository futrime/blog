---
title: NanoPi NEO上手评测（二）——性能评估
date: 2019-07-28
draft: false
author: Futrime
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - NanoPi NEO
  - 性能
---


在我上次的博客中，我简洁地介绍了NanoPi NEO并给NanoPi NEO装了系统。今天就进入第二阶段：NanoPi NEO性能评估。话不多说，先SSH连接NanoPi NEO：

![登入界面.png][2]

首先下载UnixBench：

    wget http://175.6.32.4:88/soft/test/unixbench/unixbench-5.1.2.tar.gz

然后解压

    tar -xzvf unixbench-5.1.2.tar.gz

    cd unixbench-5.1.2

显然不需要进行图形测试或者不在图形化界面下测试，将Makefile文件中GRAPHICS_TEST = defined注释掉，我的是在46行。

![UnixBench配置.png][3]

还要注意UnixBench需要![Perl][4]和[gcc][5]，FriendlyCore自带了。

随后执行 `make` 命令。

![UnixBench make.png][6]

有一大堆warning，大概是因为软件是2007年开发的，和现在的GCC有些不兼容，但是不影响使用。

随后运行 `./Run` ，需要时间比较长（毕竟Cortex-A7），耐心等待一下吧。

![UnixBench运行.png][7]

似乎连接断掉了。

![连接超时.png][8]

看来NanoPi NEO的网络稳定性不够。那就用 `nohup` 吧。

    nohup ./Run

大概等了半个小时，测试才算是完成了。UnixBench目录下有文件夹results，里面存放着跑分记录，甚至有一个已经编译成HTML的，方便起见，我将其编译成PDF导出：[Benchmark of NanoPi-NEO _ GNU_Linux on Thu Jul 25 2019.pdf][9]

![Allwinner H3][10]

对比Intel® Celeron™ J3455的跑分：[PDF][11]

![J3455单核][12]

全核心跑分是491分，已经比Intel® Celeron® J3455一个核心高了。据说全核心分数达到500分就是合格的VPS了，看来这台机当VPS也没多大问题。

再说说发热，NanoPi NEO的发热是真的厉害，虽然有散热片但是完全压不住。跑UnixBench的时候要外接一个风扇才能避免频繁降频，毕竟全志H3是40 nm制程的，也不能要求太多。

# 超频

超频？做梦吧。

不过用 `cpu_freq -s 1200000` 可以把频率锁死在1.2GHz，对比起原来的默频864MHz，up-to 1.08GHz还是有很大性能提升的，当然发热也很大提升了。

顺带说一下，用 `cpu_freq` 不仅可以超频，还可以降频，在NanoPi NEO恶劣的散热环境下，如果没有性能追求，可以适当降频，以达到降温提效的作用。

  [2]: https://futrime.gitee.io/image-cdn/2019/07/3017096555.png
  [3]: https://futrime.gitee.io/image-cdn/2019/07/3850255125.png
  [4]: https://www.perl.org/
  [5]: https://gcc.gnu.org/
  [6]: https://futrime.gitee.io/image-cdn/2019/07/2276911656.png
  [7]: https://futrime.gitee.io/image-cdn/2019/07/3256661076.png
  [8]: https://futrime.gitee.io/image-cdn/2019/07/4262360113.png
  [9]: https://futrime.gitee.io/image-cdn/2019/07/2631931879.pdf
  [10]: https://futrime.gitee.io/image-cdn/2019/07/164453566.png
  [11]: https://futrime.gitee.io/image-cdn/2019/07/4246658478.pdf
  [12]: https://futrime.gitee.io/image-cdn/2019/07/1702039088.png
