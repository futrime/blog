---
title: Jupyter Notebook的安装和常见配置
date: 2020-02-10 10:43:00
draft: false
author: Futrime
image: https://futrime.gitee.io/image-cdn/2020/02/163474455.png
description: Jupyter Notebook是一个开源Web应用程序，可让您创建和共享包含实时代码，方程式，可视化效果和叙述文本的文档。
categories:
  - 奇技淫巧
  - 经验杂谈
tags:
  - Jupyter Notebook
  - 安装
---


Jupyter Notebook是一个开源Web应用程序，可让您创建和共享包含实时代码，方程式，可视化效果和叙述文本的文档。用途包括：数据清理和转换，数值模拟，统计模型，数据可视化，机器学习等。相比于IDLE、PyCharm、Visual Code等IDE，它具有适用性广（能上网就可以用）、简洁易用等优势。目前它和它的衍生物已经覆盖了广大爱好者领域以及科研领域。这篇文章将会介绍Jupyter Notebook的安装和一些常见的配置方法。

### 安装Jupyter
在安装Jupyter之前，请确保Python和PyPI环境已经成功安装。安装教程请参考我的文章：[Linux下搭建Python环境][2]。

官方提供了conda、pip、pipenv和Docker四种方式安装。经过我的实测，使用pip安装的方式最为简洁方便而且轻量无污染。使用如下命令即可安装最新版本Jupyter Notebook:
```
pip3 install --upgrade pip
pip3 install jupyter
```
如果提示没有 `pip3` ，请参考：[Linux下搭建Python环境][3]。
如果提示 `Permission Denied` 请在命令前加上 `sudo `，下文都是如此。
这样子Jupyter Notebook就安装完成了，我们可以使用如下命令启动：
```
jupyter notebook
```
### 配置公网访问
往往我们的Jupyter Notebook是搭建在服务器上的，那么我们就需要开放公网的访问权限。输入命令如下生成配置文件：
```
jupyter notebook --generate-config
```
编辑文件 `~/.jupyter/jupyter_notebook_config.py`，查找 `#c.NotebookApp.port =`，修改为
```python
c.NotebookApp.port = 80
```
`80`可以是任何你想要使用的端口，注意如果使用国内服务提供商并且解析到服务器的域名没有备案就不可以使用80、443和8080端口。

查找 `#c.NotebookApp.ip =`行，修改为
```python
c.NotebookApp.ip = '*'
```
就可以被所有公网设备访问，另外也可以将`*`修改为指定的IP。

查找 `#c.NotebookApp.open_browser =`行，修改为：
```python
c.NotebookApp.open_browser = False
```
以避免自动打开浏览器。

### 修改Jupyter根目录
查找 `c.NotebookApp.notebook_dir =` 行，修改为
```python
c.NotebookApp.notebook_dir = '<目录>'
```

### HTTPS访问配置
现在HTTP协议已经逐渐落伍了，取而代之的是采用了TLS或SSL加密的HTTPS协议。配置HTTPS前我们首先需要对自己域名申请到证书，并且取出里面的公钥文件（腾讯云是/nginx/xxx.crt)和私钥文件（腾讯云是/nginx/xxx.key)并上传到服务器上。

查找 `#c.NotebookApp.keyfile =`行，修改为
```python
c.NotebookApp.keyfile = <私钥路径>
```

查找 `#c.NotebookApp.certfile =`行，修改为
```python
c.NotebookApp.certfile = <公钥路径>
```

### 启动Jupyter Notebook
完成以上配置后，我们使用以下命令实现后台启动、静默运行、记录日志和提供服务：
```
nohup jupyter notebook  > jupyter.log 2>&1 &
```
如果使用了sudo或者root用户的话要在`notebook`后`> jupyter.log`前加上 ` --allow-root `。

参考链接：
1. https://jupyter.org/
1. https://jupyter.org/install
1. https://jupyterlab.readthedocs.io/en/stable/getting_started/installation.html
1. https://jupyter.readthedocs.io/en/latest/projects/config.html

  [2]: https://blog.futrime.com/hacking/131.html
  [3]: https://blog.futrime.com/hacking/131.html
