---
title: 上传代码至coding.net
date: 2018-05-07 09:54:15
tags:
 - git
 - Node
---

### 安装git已经生成key

需要正确安装git，配置用户名邮箱以及生成key

### 创建coding.net项目

登录coding.net创建私人项目

并在项目设置-部署公钥 中添加key

### 上传

如果在coding.net创建时已经生成README.md则需要先合并代码

初始化git

```
git init
```

编辑.gitignore文件，忽略上传node_modules和dist文件夹

```
.DS_Store
node_modules/
/dist/
```

添加代码至仓库

```
git add .
git commit -m "first commit"
```

上传代码至远程仓库

```
git remote add origin git@xx.xx.xx.xx:repos/xxx/xxx/xxx.git
git push -u origin 分支名
```

如果出现错误，大概是因为已经在远程仓库生成了README.md文件，需要先合并代码

```
git pull --rebase origin master
```

然后重复上一步提交

[参考文章](https://blog.csdn.net/gaoying_blogs/article/details/53337112)