---
title: 今天咋做菜？让AI来告诉你！
date: 2020-05-05 08:35:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/04/860835759.png
description: 疫情期间宅在家里总是要做饭（虽然已经过去了），每天做同样的菜倍感厌烦，想自己做个新的菜，又苦于不会做。现在，一个（近乎）完美的解决方案来了：让AI帮你生成菜谱。
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - AI
  - 机器学习
  - 深度学习
  - RecipeGPT
  - GPT
  - 菜谱
---

RecipeGPT是一个利用GPT模型（就是OpenAI在年初开放的那个两亿参数模型），用编故事的形式生成菜谱的网站。我们打开这个网站：

![Screenshot_2020-04-07 RecipeGPT Generative Pre-training Based Cooking Recipe Generation and Evaluation System.png][2]

网站很简单且只有两个功能，左边的是生成制作方式,右边的是根据你想做的东西，在你现有的材料中选出原材料（没啥用）。点左边的。

![Screenshot_2020-04-07 RecipeGPT Generative Pre-training Based Cooking Recipe Generation and Evaluation System(2).png][3]

在 `Recipe Title` 栏中输入你想做的东西的名字（要用英文，一行一个），在 `Recipe Ingredients` 栏中输入你想做这个东西要用的材料。随后点击 `Generate` 。

![Screenshot_2020-04-07 RecipeGPT Generative Pre-training Based Cooking Recipe Generation and Evaluation System(1).png][4]

等待片刻，食品的制作方式就可以显示出来了。

![Screenshot_2020-04-07 RecipeGPT Generative Pre-training Based Cooking Recipe Generation and Evaluation System(3).png][5]

有一说一，生成的菜谱有种比较千篇一律的感觉，而且都是偏欧美的菜谱，大概是模型规模比较小而且训练数据主要是欧美菜谱。不过，对于在一个公开免费的网站上面的东西来说，质量很不错了。

再说说右边那个的功能。右边那个是当你输入你想做的东西以及你家里有的所有原材料时，为你生成需要使用的东西，然后可以复制去用左边那个去生成，这样效果好一些。

  [2]: https://futrime.gitee.io/image-cdn/2020/04/1186107588.png
  [3]: https://futrime.gitee.io/image-cdn/2020/04/388028716.png
  [4]: https://futrime.gitee.io/image-cdn/2020/04/4251848756.png
  [5]: https://futrime.gitee.io/image-cdn/2020/04/1123247175.png
