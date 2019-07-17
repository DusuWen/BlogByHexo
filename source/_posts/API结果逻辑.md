---
layout: 'tvshowvue'
title: API结果逻辑
date: 2018-06-28 11:05:48
tags:
 - API
---

## 准备过程

* tmdb的key
* track.tv的key

### 获取下集播放时间的方法

* 先用tmdb的接口获取imdb_id
* 再用trakt的接口获取下集的季数和集数以及名称
* 在tmdb用季数和集数获取该集的播放日期
* 使用播放日期与当前日期进行计算得出倒数日
* 使用tvdb获取airs days of week和airsTime

### 问题

* 期间使用了imdb的id，有些剧集没有此id，导致调用过程中断
* 播放日期仅精确到日，并非app中的时间
* 播放日期的时区与当前使用者时区并不相同
* tmdb与tvdb返回的时间时区与使用者不相同

### tmdbAPI使用方法

* 在官网注册登录之后，创建应用，生成apikey
* 每次调用接口需要在get请求中戴上apikey

```
https://api.themoviedb.org/3/tv/1415?api_key={you_apikey}&language=en-US
```

### tmdb获取用户信息

* 创建request_token

```
https://api.themoviedb.org/3/authentication/token/new?api_key={you_apikey}
```

* 返回示例

```
{
  "success": true,
  "expires_at": "2016-08-26 17:04:39 UTC",
  "request_token": "ff5c7eeb5a8870efe3cd7fc5c282cffd26800ecd"
}
```

* 获取用户授权后的request_token
* 需要跳转至tmdb的官网，用户登录授权后，回调至所填地址

```
https://www.themoviedb.org/authenticate/{REQUEST_TOKEN}?redirect_to=http://www.yourapp.com/approved
```

* URL中的request_token用在接下来获取session_id

```
https://api.themoviedb.org/3/authentication/session/new?api_key={you_apikey}&request_token={request_token}
```

* 返回示例

```
{
  "success": true,
  "session_id": "79191836ddaa0da3df76a5ffef6f07ad6ab0c641"
}
```

* 根据apikey和session_id即可获取用户信息

```
https://api.themoviedb.org/3/account?api_key={you_apikey}&session_id={session_id}
```

#### 判断时间使用moment.js



###	Trakt获取授权的流程

* 使用/oauth/authorize方法

38b6c1061456505ba0f4d24a68962ab34623b7faf736cbb413490ece3da64aa0
