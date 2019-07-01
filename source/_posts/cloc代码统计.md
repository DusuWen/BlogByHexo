---
title: cloc代码统计
date: 2019-06-12 22:12:35
tags:
---

## Cloc简介

Cloc是一款使用Perl语言开发的开源代码统计工具，支持多平台使用、多语言识别，能够计算指定目标文件或文件夹中的文件数（files）、空白行数（blank）、注释行数（comment）和代码行数（code）。

## 安装和环境需求

首先Node.js和Npm环境比不可少，其次需要Perl环境，可以在[ActiveState](https://www.activestate.com/products/activeperl/downloads/)中免费下载安装

最后就是执行`$ npm install -g cloc`全局安装cloc

## 使用

详细命令可以查看官方文档[cloc](https://github.com/kentcdodds/cloc#readme)

例如在Vue工程中统计src目录下的文件和代码可以直接执行`cloc src\`，就可以看到src路径下所有文件和目录里的文件类型以及数量，代码行数、空格行数还有注释行数





