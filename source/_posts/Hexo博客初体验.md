---
title: Hexo博客初体验
date: 2018-05-04 15:53:35
tags:
 - Hexo
---

# #方案

使用git+hexo部署到github

### 配置git

假设已经安装git，但是忘记有没有生成公钥

在windows桌面右键 git bash here

进入ssh目录

```
cd ~/.ssh
```

没有该文件夹存在的话就是没有生成过

先配置用户名密码，然后生成私钥公钥

```
git config --global "username"
git config --global "email@address"
ssh -T git@github.com
```

将公钥添加至github中的ssh key管理中

### Github新建仓库

登录github，新建一个仓库，并保存git地址

### 安装Hexo

默认已经安装node

执行

```
npm install -g hexo-cli
```

安装完成后，在指定文件夹执行新建和安装依赖操作

```
hexo init <folder>
cd <folder>
npm install
```

安装完成后，需要按照文档配置config.yml

配置部署地址

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: #上一环节保存的github仓库地址
  branch: master # github分支
```

### 使用Hexo

安装插件

```
npm install hexo-deployer-git --save
```

新建一篇文章

```
hexo new [layout] "新文章标题"
```

如果没有设置 `layout` 的话，默认使用 [_config.yml](https://hexo.io/zh-cn/docs/configuration.html) 中的 `default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。 

生成静态文件

```
hexo g
```

部署网站

```
hexo d
```

