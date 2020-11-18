---
title: hash和history
date: 2020-11-17 16:54:21
tags:
---

在单页应用(SPA)中会使用前端路由，页面地址的跳转都是在浏览器端完成的，不会重新请求服务端获取 html，html 只在应用初始化时加载一次。所有的页面由不同的组件构成，页面的切换其实就是不同组件的切换。
<!-- more -->
### hash

hash 就是 URL 中的锚点。

例如 http://yoursite.com/#/user/id 这样的 URL，是利用 hash 模拟出来的完整 URL，当 URL 改变时，页面不会重新加载。

hash 是一个真实的 URL。

### history

如果不想要 URL 中的 hash，可以利用 `history.pushState` API 来完成 URL 跳转而无需重新加载页面，但是需要后台的配置支持。

history 是一个虚假的 URL。
