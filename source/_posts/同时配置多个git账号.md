---
title: 同时配置多个git账号
date: 2019-09-06 11:20:37
tags:
	- git
---

### Why

因为要在公司电脑上使用公司的gitlab同时还需要使用自己的github，所以需要根据项目区分commit时的账户。

#### 生成ssh秘钥

仅配置github的秘钥，打开.ssh文件夹，

* 执行`ssh-keygen -t rsa -C "github邮箱" -f ~/.ssh/github_rsa`
* 会生成github_ras和github_rsa.pub
* 把github_rsa.pub中的内容配置到github中
* 进入.ssh路径配置一个config

```
Host github.com
    HostName github.com
    User userName
    IdentityFile ~/.ssh/github_rsa
```

#### 测试连接

在.ssh路径下执行`ssh -T git@hostName`

```
$ ssh -T git@github.com
Warning: Permanently added the RSA host key for IP address '13.250.177.223' to the list of known hosts.
Hi DusuWen! You've successfully authenticated, but GitHub does not provide shell access.
```

#### 配置git仓库

##### 全局配置

因为公司gitlab使用较多，所以把公司账号配置为全局

```
git config --global user.name 'userName' #公司账号名称
git config --global user.email 'userName@companyName.com' #公司账号邮箱
```

##### 仓库配置

因为在公司电脑上运行的个人项目只是少数，所以一般把项目从github上clone下来后，通过webstorm中terminal在当前项目设置local账号

```
git config --local user.name 'username' #github账号名称
git config --local user.email 'username@gmail.com' #github账号邮箱
```

#### Ending

通过这次配置，普通的公司项目默认使用gitlab账号push代码，而个人从github拉取的项目会使用github账号push代码。缺点是每次clone项目下来需要进行一次本地仓库配置。

参考：[如何在同一台电脑上使用github和gitlab]()
