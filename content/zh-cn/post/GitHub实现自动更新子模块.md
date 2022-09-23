---
author: Futrime
title: GitHub实现自动更新子模块
description: 当我们的仓库中使用到子模块，并且需要保持最新版本时，可以使用GitHub Actions实现自动更新。
date: 2022-09-23
categories:
    - 经验杂谈
tags:
    - GitHub
    - 子模块
    - 自动化
    - GitHub Actions
image: https://i.ibb.co/fCG5XK4/hutao-ls-f-970x350.jpg
draft: false
---

## 导言

在开发较大规模的项目时，为了管理方便，我们通常会将项目拆分成多个子模块，使用不同的仓库进行管理，并使用Git的子模块（Submodule）功能引入到同一个主仓库中。

但是，由于不同的仓库分别管理提交记录（Commit）和版本，我们需要在引用的仓库中持续更新，以保证各个模块最新。在传统的版本管理中，我们需要将仓库拉取到本地，手动运行更新命令，然后再提交和推送。这种管理方式比较麻烦，而且手动操作有可能产生意料之外的错误。

GitHub Actions是一个基于GitHub仓库和Git工具的持续集成和部署自动化工具。直观来说，GitHub Action就是负责在某个事件发生时，在某个特定环境下，进行某些特定操作序列。

![GitHub Actions](https://i.ibb.co/vzkHggS/LUYjmo-UFu.png)

GitHub Actions运行的基本单元是工作流（Workflow），由`.github/workflows/`下的配置文件实现。在每一个工作流配置文件中，主要包含两部分：事件触发器和工作任务。工作任务部分可以包含多个任务（Job），每个任务包含多个步骤（Step）。需要注意的是，在每一个任务中，所有步骤必须在同一个环境下运行；而不同的任务间环境（包括文件）并不共享，而需要使用缓存（Cache）或成品（Artifact）进行共享。这也是GitHub Actions区分任务和步骤的原因。

## 方法

需要在仓库内的`.github/workflows/`目录下建立一个`.yml`文件，不妨命名为`update_submodules.yml`。需要注意的是，仅有默认分支下的工作流可以被执行，所以当你在其他分支修改工作流配置文件后，请同步到默认分支。

先给工作流命个名吧，在`update_submodules.yml`写入以下内容：

```yml
name: Update Submodules
```

### 事件触发器

我们定义两个事件触发器：手动运行和每15分钟自动运行。在`update_submodules.yml`写入以下内容：

```yml
on:
  schedule:
    - cron: "* * * * *"
  workflow_dispatch:
```

`schedule`事件触发器用于定时执行任务。其中的`cron`参数配置和Linux下`cron`命令配置文件格式一致。但需要注意的是，GitHub允许定时执行的最高频率是15分钟，且不保证一定能够运行。

`workflow_dispatch`事件触发器用于在Actions菜单中添加一个手动运行按钮。

### 工作任务

由于我们只需要一个环境，因此只需要一个任务。在`update_submodules.yml`写入以下内容：

```yml
jobs:
  update_sdk:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Update submodules
        run: |
          git submodule update --init --remote
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git commit -am "Update SDK" || exit 0
          git push || exit 0
```

在第一个步骤（Checkout）中，我们签出仓库的默认分支，这类似于在本地运行`git clone`命令。

在第二个步骤（Update submodules）中，我们运行一系列命令，实现更新子模块、提交、推送。这里利用到一个有趣的特性，那就是GitHub Actions环境中必然有能读写仓库的密钥，因此不需要另外配置密钥。

保存、提交并推送，很快就能看到自动更新的运行效果了。

## 参考资料

1. [Git - git-submodule Documentation](https://git-scm.com/docs/git-submodule)
2. [GitHub Actions Documentation - GitHub Docs](https://docs.github.com/en/actions)
3. [Events that trigger workflows - GitHub Docs](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
4. [Workflow syntax for GitHub Actions - GitHub Docs](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)