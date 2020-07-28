---
title: Vue 骨架图
date: 2018-05-22 14:20:44
tags:
 - Vue
---
### 减少首屏空白的骨架图

使用[这篇文章](https://vv-ui.github.io/VV-UI/#/giud "这篇文章")介绍的UI框架以及内置的骨架图组件
<!-- more -->
### 实际开发
VVUI的作者在介绍了两种骨架图使用方法，第一种是预加载，第二种的SSR.SSR服务端渲染比较难，所以使用了作者的UI框架并且实现基本效果。有一点需要注意的是虽然作者的github和官网上均已加入了骨架图组件和使用方法介绍，但是npm上的版本依旧是旧的1.0.4.而不是最新的1.1.0，所以出现引用的skeleton组件不存在的问题

### 解决方法
自己写一个组件，加一个动画上去就可以了。

### 未来优化
该UI作者实现预渲染使用的是[https://github.com/chrisvfritz/prerender-spa-plugin](https://github.com/chrisvfritz/prerender-spa-plugin "prerender-spa-plugin")插件，所以想要更加快速轻便的使用预渲染，还要精度预渲染插件文档。
