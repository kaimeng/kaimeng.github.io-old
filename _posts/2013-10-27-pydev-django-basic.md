---
layout: post
title: "使用 pvDev 开发 Django 项目"
description: "使用 pvDev 开发 Django 项目"
tags: [技术, Django]
comments: true
share: true
---

自从第一次接触Python，它就成了我一直最喜欢的语言。简练的语法，丰富的第三方插件，强大的数据处理能力，成为现在很多人工智能项目首选的语言。

pyDev 是 Eclipse 下的 Python 开发插件，可以使用该插件在Eclisp上开发 python 项目,其中集成了对 Django 的支持。

## 安装

1. 安装 [django](https://docs.djangoproject.com/en/1.5/topics/install/)
2. 安装 pyDev，具体安装就不介绍了，去问伟大的 Google 。建议先将插件下载下来之后离线安装，在线安装我试了几次总是不成功。

## 新建项目

好像 pyDev 有 bug，在没有 pyDev 项目时无法建立 django 项目。所以按照下面的步骤新建项目：

1. 随便建立一个普通的 pyDev 项目
2. 新建 Django 项目

新建好之后试运行一下，右击项目，Run as --> PyDev:Django，在浏览器中输入 http://127.0.0.1:8000 看到页面显示 It worked! 说明项目新建成功了。

## 项目开发

直接参考 Django 的[官方教程](https://docs.djangoproject.com)

开发过程中 cmd 进入你的目录，使用 cmd 和 pyDev 同时进行操作，这样会使很多工件非常方便。
在 pyDev 中进行代码的开发，在 cmd 中进行数据库，新建 app 之类的操作。

同步数据库和代码

    python manage.py syncdb
    
新建 app

    python manage.py startapp yourAppName
    
查看 app 的 sql 代码

    python manage.py sql yourAppName