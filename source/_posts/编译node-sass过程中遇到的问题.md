---
title: 编译node-sass过程中遇到的问题
date: 2019-03-01 13:51:57
tags:
---
### 在项目暗转过程中需要编译node-sass以提供sass转css的支持
#### 但是使用环境没有vc++2015和python2编译环境会导致node-sass编译不通过
安装编译工具

`npm install -g node-gyp`
<!-- more -->
之后安装
`npm install --global --production windows-build-tools`

如果编译还不成功可以修改vc++库的版本

`npm config set msvs_version 2015 --global`
