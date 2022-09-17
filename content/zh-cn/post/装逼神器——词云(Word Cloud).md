---
title: 装逼神器——词云(Word Cloud)
date: 2019-09-22
description: WordCloud词云是一种数据可视化形式。
image: https://futrime.gitee.io/image-cdn/2019/09/1151820277.jpg
draft: false
author: Futrime
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - 改编
  - Python
  - 词云
---


[WordCloud词云][1]是一种数据可视化形式

* 不会的时候，感觉很厉害
* 会用了之后，感觉别人都玩烂了

这篇文章就教大家如何使用Python生成词云

### 准备工作

先要安装wordcloud（本体），matplotlib（绘图），jieba（中文分词），PIL（依赖）这些库。

使用pip安装：
    pip install wordcloud matplotlib jieba PIL
### WordCloud()函数介绍

`WordCloud()` 的参数如下：

* `font_path` ：可用于指定字体路径，包括otf和ttf
* `width` ：词云的宽度，默认为400
* `height` ：词云的高度，默认为200
* `mask` ：蒙版，可用于定制词云的形状
* `min_font_size` ：最小字号，默认为4
* `max_font_size` ：最大字号，默认为词云的高度
* `max_words` ：词的最大数量，默认为200
* `stopwords` ：将被忽略的停用词，如果不指定则使用默认的停用词词库
* `background_color` ：背景颜色，默认为black
* `mode` ：默认为RGB模式，如果为RGBA模式且background_color设为None，则背景将透明

### 从零开始

* 导入库

    from wordcloud import WordCloud
    import matplotlib.pyplot as plt
Matplotlib是一个Python 2D绘图库，可以生成各种硬拷贝格式和跨平台交互式环境的出版物质量数据。[官方网站][3]

* 生成WordCloud

    wc = WordCloud().generate(open('input.txt').read())
这里的 `input.txt` 可以是一篇文章、一本书，甚至无序的数字序列，WordCloud将会根据词频自动输出。

如果使用Linux无界面操作系统或者Jupyter Notebook，中文有可能会出现无字体的问题，那么就需要自定义字体路径：

    wc = WordCloud(font_path='font.ttf').generate(open('input.txt').read())
其中font.ttf是自定义的字体路径，建议放在同目录下。


* 显示图片

这里用到了万能的Matplotlib，可以在有GUI的环境下或者Jupyter Notebook下预览图片：

    plt.imshow(wc, interpolation='bilinear')
    plt.axis('off')
    plt.show()
Matplotlib在Jupyter Notebook有时候会出现调用时只显示对象摘要不输出图片的问题，这个时候重新执行一次就可以了，效果如下：

![v2-3ef01c6602a5a1257d0d83d8d1c004b0_hd.jpg][4]

* Chinese Trick

你可能注意到了，这个时候词语好像不太对劲，本来好好的“行者”硬是被分成了“行者道”。这是因为WordCloud本身并不是专门为中文适配的，所以我们要祭出[jieba][5]来给中文分词：

    import jieba
    text = ' '.join(jieba.cut(text))
到此，Word Cloud就告一段落啦，是不是十分简单？

参考链接：
[03 高端又一般的词云][6]
[Matplotlib][7]
[结巴][8]
[WordCloud][9]


  [1]: http://amueller.github.io/word_cloud/index.html
  [3]: https://matplotlib.org/
  [4]: https://futrime.gitee.io/image-cdn/2019/09/4146082687.jpg
  [5]: https://github.com/fxsjy/jieba
  [6]: https://zhuanlan.zhihu.com/p/44165235
  [7]: https://matplotlib.org/
  [8]: https://github.com/fxsjy/jieba
  [9]: http://amueller.github.io/word_cloud/index.html
