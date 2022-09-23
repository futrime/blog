---
title: Python多维列表的一些坑
date: 2020-06-20
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/05/3733930615.png
categories:
  - 玩机日志
  - 经验杂谈
tags:
  - Python
  - 列表
---

近日在某个程序中用到了二维动态规划，本来使用一维数组模拟二维数组，但是感觉不够直观，便打算改成二维数组。我用了以下代码来创建：

```
a = [[-1] * 100] * 10000]
```

但是发现运行同样的算法，得出的结果却截然不同，一直找不到原因。后来我的同学用IDLE实时查看了列表的情况，发现在每次对某个元素进行修改时，所有第二维度的列表都会被修改。例如：

```
a[0][1] = 5
print(a[0][2])
# Output: 5
```

后来查找Python文档找到了以下条目：

![企业微信截图_15882968065453.png][2]

在Python中，当我们使用 * 号复制列表的时候，复制的仅仅是指向列表实体的指针，而不是实体本身。因此，如果真的需要创建多维数组，我们应当使用如下代码：

```
a = [[-1 for i in range(100)] for i in range(10000)]
```

参考链接：
1. https://docs.python.org/zh-cn/3/library/stdtypes.html

  [2]: https://futrime.gitee.io/image-cdn/2020/05/3320206389.png
