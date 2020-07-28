---
title: 安装MongoDB
date: 2018-06-15 09:35:05
tags:
 - mongodb
---

## 安装MongoDB

[参考链接](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/)

启动命令

```
C:\Program Files\MongoDB\Server\3.4\bin\mongo.exe 
```

停止命令

```
Control+C
```
<!-- more -->
创建的目录

```
mkdir c:\data\db
 mkdir c:\data\log
```

启动MongoDB

```
net start MongoDB
```

数据库储存的位置

```
C:\data
```

 

如果启动服务时报错1067

在任务管理器中看有没有占用端口的程序，杀掉

命令行查看所有端口的占用情况

```
netstat -ano
```

