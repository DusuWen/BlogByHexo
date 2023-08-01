---
title: 关于browserslist的一点学习
date: 2023-06-30 10:30:57
tags: 
    - npm
---

### 什么是browserslist

`browserslist`是一个配置工具，通过配置查询条件可以筛选出内置数据库 `caniuse-lite`里的符合条件的浏览器版本，对于需要做兼容性适配的项目来说，是一个非常有用的工具，`browserslist`通常搭配`babel`来使用，在得到需要兼容的浏览器版本后，`babel`会计算出最合适的`垫片`用来转译目标浏览器不支持的es6语法
<!-- more -->

### 如何配置

通常可以直接在`package.json`中增加一个字段

```json
{
  "browserslist": [
    "last 1 version",
    "> 1%",
    "maintained node versions",
    "not dead"
  ]
}
```

或者在项目根路径创建`.browserslistrc`文件

```
last 1 version
> 1%
maintained node versions
not dead
```

`last 1 version` 表示最新的1个版本

`> 1%` 表示使用率超过1%

`maintained node versions`表示官方还在维护的node版本

`not dead`表示官方还在维护

* 如果开发者没有指定配置，`browserslist`会使用默认配置

### 查看浏览器列表

在项目路径下执行`npx borserslist`可以得到配置文件中的查询条件在`caniuse-lite`库中查询后的结果

```
and_chr 114
and_ff 113
and_qq 13.1
and_uc 13.4
android 114
baidu 13.18
chrome 114
chrome 113
chrome 112
chrome 109
edge 114
edge 113
edge 112
firefox 114
firefox 113
ie 11
ios_saf 16.5
ios_saf 16.4
ios_saf 16.3
ios_saf 16.1
kaios 3.0-3.1
op_mini all
op_mob 73
opera 99
opera 98
safari 16.5
safari 16.4
samsung 21
samsung 20
```

### 有什么问题

如果没有设置配置文件，或者配置文件采用`> 1%`这种写法，随着项目越来越久，会因为一个意料外的问题，假设我们的项目是诞生于2020年，那时`npm i browserslist`后在`package.json`中的版本是`^4.5.0`，这个版本的`browserslist`内置的`caniuse-lite`库也是一个适配的版本，这个时候`> 1%`的语法查询出来的最低的chrome支持版本可能是109，但是2023年某一次删除`node_modules`和`package-lock.json`后再`npm install`后，根据`package.json`中的`^4.5.0`安装得到的`browserslist`依赖的版本可能是4.6.0，那么对应的`caniuse-lite`库也得到了更新，所以查询出来的chrome最低支持版本上升到了112，也就是说2年过去了，chrome支持的最低版本从109上升到了112，那么就会产生一个严重的问题：我们的项目必须要支持109，但是现在查询得到112，那么`babel`就不会再转移109不支持的语法，导致109实质上被放弃支持，这与我们的初衷有所背离。

### 怎么解决

所以，如果项目有明确的指出需要支持什么样的浏览器的最低版本，一定要在`browserslist`配置里明确的写出，比如：

```
chrome >= 109
```

那么未来不管`caniuse-lite`数据库怎么更新，这个项目都会支持到109的chrome版本。
