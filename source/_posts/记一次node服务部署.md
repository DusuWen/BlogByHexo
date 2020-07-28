---
title: 记一次node服务部署
date: 2018-05-28 13:08:37
tags: 
 - Node
---

### 前期准备

* 安装git，并且配置用户名密码生成公钥
* 安装node，并且配置好环境变量
* 开发环境上传代码至云仓库，并且配置公钥
* nginx配置端口地址
<!-- more -->
### 过程

* webstorm commit代码到coding
* liunx git clone 项目git路径
* npm install安装项目
* 全局安装pm2
* 用git pull获取最新代码，用pm2管理服务状态
