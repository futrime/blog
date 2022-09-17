---
author: Futrime
title: ReadTheDocs快速上手
description: 难道ReadTheDocs就只能用来写项目文档么？当然不是咯！
date: 2021-01-31T08:08:39Z
categories:
    - 经验杂谈
    - 奇技淫巧
tags:
    - ReadTheDocs
    - 文档
    - 创作
    - GitBook
    - Sphinx
image: https://i.loli.net/2021/01/31/xmRWTtY8ln35wHf.png
draft: false
---

不想听我BB，请直接[跳转到正文](#正文)

## 背景

去年暑假，在B站刷了《关于我转生变成史莱姆这档事》和《因为太怕痛就全点防御力了》这两部番，有点中毒。因为学校8月2日就开学了，当时毒性未退，便在网上找了这两部番的小说原著电子书，带回去慢慢看。结果因此中毒反而更深了。

后来就想着，既然中毒已深，那就破罐子破摔吧，索性让自己中毒中得更加深一点，便打算自己写这两个作品的同人文章。

可是，问题在于，到底在什么平台写文章比较好呢？

一开始打算直接在哔哩哔哩作者后台、刺猬猫作者后台这一类投稿平台的文本编辑器上面直接书写，但是却想到，在这些平台书写，每次想要写，都必须进行麻烦的登录、前往后台、前往草稿箱等一系列操作，等到操作完，写作的意头就已经丧失殆尽了。

某天偶尔看到有个有趣的网站，叫做[GitBook](https://www.gitbook.com/)，试用了一下，发现挺好用的。而且可以绑定自定义域名，构建一个专门的同人文阅读站点。不过遗憾的是由于其使用了Fastly的CDN，在国内访问质量极其差，必须手动使用Cloudflare的CDN，这导致了访问速度缓慢。当时觉得这个网站实在太赞了，很难想到还有什么更加优秀的解决方案。

问题出在写到大概第十话的时候。由于前文越来越多，掌握情节走向变得有点困难。我便打算将所有文字下载下来，制作成EPUB或者PDF，放在自己的小米多看阅读器，时常翻阅。这时我发现，GitBook个人免费版是不支持导出PDF的，而EPUB的导出，连按钮都没有。官方说认证成为社区开发者，就可以导出，但是这个又不是某个开源软件的文档，必然不可能申请通过。

所幸GitBook出身于一个叫做gitbook-cli的命令行工具，在适当的情况下，应该是可以通过这个工具导出的。理论上，实践是简单的；实际上，坑却到处都是：gitbook-cli工具已经有很长时间没有维护了——GitBook的开发者们，可能在盈利或其他的目的指导下，已经将GitBook转化为了一个在线服务提供商，而不是开源软件的开发组了。因此在使用gitbook-cli工具时，即便是安装，也会屡屡报错。最后发现是因为NodeJS版本太新了，必须要降级到NodeJS v10.x.x才能正常使用，这就和其他软件不兼容了。虽然有NVM这一类工具，但是又得花很多时间折腾。另外一点，gitbook-cli导出PDF、EPUB等并不是原生支持的，而是要使用Calibre的库，而在Linux下安装Calibre，又没得指定加速代理，下载速度龟速——甚至睡了一觉都还没装好。总之种种原因，实在将我劝退了。

直到去年12月，仔细研读了ReadTheDocs的文档之后，才发现，原来最好的工具——强大、易用、开源的工具——始终在我身边。

## 站点设置

终于来到正文部分了。首先要注册一个账户。

打开[已连接的服务](https://readthedocs.org/accounts/social/connections/)，在GitHub、GitLab、Bitbucket中至少选择一个连接。在对应的连接账户中，必须要创建用于放置文档的仓库。

打开[导入代码库](https://readthedocs.org/dashboard/import/)，导入刚才建立的仓库。按照指示即可建立。

项目创建完成后，可以在 管理 栏增加自定义的 域 。ReadTheDocs的速度比较快，不需要再加上Cloudflare加速了。

在 高级设置 栏里面可以设置构建方式。勾选 `Build pull requests for this project` 可以实现实时构建。

“Python配置文件”栏可以设置Sphinx配置文件conf.py所在的地方，默认情况下是项目根目录下的conf.py文件。通过这个，可以实现仅构建某个文件夹的内容。

在下面的“文档类型”中可以选择使用Sphinx或者是Mkdocs构建。如果只是使用网页版，两种都可以，Mkdocs初步配置简单一点；但是如果想要构建Epub、PDF等离线文档，就必须使用Sphinx。踩了Mkdocs的坑，在此个人建议使用Sphinx，本文只介绍使用Sphinx构建。

## 初始化操作

首先在“高级设置”栏中修改文档类型为“Sphinx Html”，并且在“所需求的文件”填入 `requirements.txt` 。

进行在线构建，必须要在本地进行预先配置。使用pip安装Sphinx并初始化sphinx环境（recommonmark是为了支持Markdown）：

```sh
pip install sphinx recommonmark

sphinx-quickstart
```

修改生成的conf.py，添加如下语句：

```python
extensions = ['recommonmark', 'sphinx_rtd_theme']
```

同时可以修改project、author、copyright等内容，使用中文的话还要修改language。为了使用主题（比如ReadTheDocs的典型风格主题），还要修改html_theme.以我的为例：

```python
project = '我的文档'
copyright = '2021, Futrime'
author = 'Futrime'

extensions = [
    'recommonmark',
    'sphinx_rtd_theme'
]

language = 'zh_CN'

html_theme = 'sphinx_rtd_theme'
```

创建requirements.txt，填写：

```
recommonmark
sphinx-rtd-theme
```

然后提交修改即可，如果配置了自动构建，此时应当已经开始构建了。

## 建立目录索引

文档的构建，需要index.rst目录索引，否则ReadTheDocs并不知道哪些文档应当被构建。

先上个例子：

```r
.. toctree::
   :maxdepth: 2
   
   docs/README.md

.. toctree::
   :glob:
   :maxdepth: 2
   :caption: 异闻录
   
   docs/anecdote/*
```

一开始建立的toctree只有一个，但是我们可以复制多几个，这样就可以实现多个文件夹了。glob标签代表的是正则匹配，例子中第二个toctree因为有通配符，必须要使用这个标签。caption是文件夹名称，缺失的话，该分区直接显示文档，不会有名称。

以我的为例，我们以后需要增加文件，只需要在 *docs/anecdote/* 里面创建就可以了，默认按照 **文件名的字母表序** 排列。标题将会读取文档内的第一个#号的内容。

## 写文档

以我的为例，在 *docs/anecdote/* 里面创建Markdown文档即可。

参考链接：

1. https://readthedocs.org
1. https://docs.readthedocs.io
1. https://www.sphinx-doc.org/
