---
title: Seaborn数据可视化笔记（一）——简单介绍和基本配置
date: 2020-06-26
description: Seaborn是基于Matplotlib的Python数据可视化库。 它提供了一个更为顶层的界面，用于绘制引人入胜且内容丰富的统计图形。
image: https://futrime.gitee.io/image-cdn/2020/06/2321446166.png
draft: false
author: Futrime
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Seaborn
  - 数据可视化
---

这是Seaborn数据可视化的思维导图，以后可以多回来看看作为参考。

在这篇博客中，我将简要介绍

## 全局环境
Seaborn依赖Pandas和Matplotlib库，所以我们先导入Pandas、Matplotlib和Seaborn库：
```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

有时候我们的数据中有日期数据，我们希望可视化图表可以按照日期先后显示，如图：

![__results___8_1.png][2]

那么我们就需要配置Pandas绘图日期转写：
```
pd.plotting.register_matplotlib_converters()
```

如果使用Jupyter Notebook，我们可能还希望配置Matplotlib显示方式。
```
%matplotlib -l # 查看可用的显示方式
%matplotlib <显示方式> # 设置显示方式
```
### 数据导入
首先我们需要将数据转为CSV格式。虽然Pandas支持Excel格式的数据导入，但是Excel本身有太多其他功能数据，因此我们获取到的东西和我们在Excel中看到的并不一样。我们需要在Excel中另存为CSV格式，如图：

![批注 2020-06-21 113440.jpg][3]

然后在Pandas中导入这个CSV文件：
```
my_file = '<文件名>.csv'
my_data = pd.read_csv(my_file , index_col='<索引列列名>, parse_dates=True)
```
后面两个参数可选。`index_col`用于指定索引列，默认为首列；`parse_dates`用于解析日期，也就是Pandas绘图日期转写。

导入数据后可以浏览下数据的大概
```
my_data.head(<行数>) # 查看导入的前几行，不指定即为5
my_data.tail(<行数>) # 后几行，不指定即为5
my_data.describe() # 查看求和、平均值、N分位数等数据统计信息
```

### 环境全局配置
先配置Matplotlib画布信息（其实不配置也可以的）：
```
plt.figure(figsize=(<长>, <宽>)) # 设置画布尺寸，均为正整数，这是个玄学
plt.title('<图表标题>')
plt.xlabel('<X轴标题>')
plt.ylabel('<y轴标题>')
plt.legend() # 显示图例
```
如图所示：

![__results___12_1.png][4]

然后配置Seaborn风格：
```
sns.set_style('<风格名>')# 其中风格名包括darkgrid、whitegrid、ticks、dark、white
```

其实默认的就挺好看了，-grid的是带上网格的，dark-的是背景稍微深色一点的，适合装逼。ticks好像和white区别不大。

参考链接：
1. https://www.kaggle.com/learn/data-visualization
1. https://seaborn.pydata.org/

  [2]: https://futrime.gitee.io/image-cdn/2020/06/2810164901.png
  [3]: https://futrime.gitee.io/image-cdn/2020/06/3253767635.jpg
  [4]: https://futrime.gitee.io/image-cdn/2020/06/544503379.png
