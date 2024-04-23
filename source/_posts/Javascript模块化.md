---
title: Javascript模块化
date: 2024-04-23 13:49:48
tags: [记录]
---

在 Javascript 的发展过程中，应对工程化的不断深入，逐渐发展出了多种模块化方案，在 Node.js 和浏览器环境中，出现了 CommonJS、AMD、UMD、CMD、ESM 等多种模块化规范。
<!-- more -->

### Node环境
#### CommonJS
* 所有代码都运行在模块作用域，不会污染全局作用域
* 通过 module.exports 暴露，通过 require 导入
* 提供动态导入
* 同步导入

```javascript
// 使用 CommonJS 导入
const express = require('express');

// 使用 CommonJS 导出
module.exports = function() {
  // Some functionality
};
```
### 浏览器环境
随着前端开发越来越复杂，在 script 中多个文件互相引用方法变量变得越来越难以组织，AMD、CMD这样的规范便应运而生。
#### AMD
* 全称 Asynchronous Module Definition，意为异步模块定义
* 采用异步方式加载模块
* 使用 require() 语句，需要回调函数
* 通过 RequireJS 和 curl.js 来实现 AMD 方案
* 浏览器不需要使用构建工具

#### CMD
* 专门用于浏览器端，模块的加载是异步的
* 整合了 CommonJS 和 AMD 规范的特点
* 在浏览器环境中使用 Sea.js 使用 CMD 规范模块
* 浏览器不需要使用构建工具

### UMD
一个模块/库的作者如果希望自己的代码同时支持 Node 端和浏览器端，而不需要提供两种规范的 js 文件的话，可以使用 UMD。
UMD 是 CommonJS 和 AMD 的糅合，会先判断是否 node 环境，再判断是否支持 AMD，如果都不是最后注册到 window 上。

### ESM
随着前端技术的发展，js 应用已经复杂到需要构建工具来组织代码，此时，ESM 成为了最方便的前端模块化方案选择。
ESM 是现代 JavaScript 的官方标准模块系统，也被最新版本的浏览器原生支持。
通过 export 导出，import 引入，node 新版本中得到支持，是静态的
```javascript
// 使用ES Modules 导入
import express from 'express';

// 使用 ES Modules 导出
export function myFunction() {
  // Some functionality
};
```
ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。将来服务器和浏览器都会支持 ES6 模块格式。

### 现状
在一个纯前端应用中，使用构建工具例如 webpack 可以直接在 js 文件中使用 CommonJS 和 ESM 方案的模块引用。
而在一个纯 Node.js 项目中，虽然最新版的 Node 已经原生支持 ESM,但是 Node 端众多的库仅提供了 CommonJS 方案，而 CommonJS 与 ESM 不能兼容，所以 Node.jS 项目中默认把 .js 文件拓展名与 CommonJS 模块关联，不可以在一个文件中同时使用 require 和 import 语法。
```javascript
const a = require("./a.js")
import { b } from "./b.js";
```
为了解决在现阶段 Node.js 中使用 ESM 的问题，Node.js 允许使用 .mjs 文件拓展名或者在 package.json 中明确指定 "type": "module" 属性来表示 ESM 模块。
或者当你的项目指定 "type": "module" 后有某个文件需要引用 CommonJS 模块，可以使用 .cjs 文件拓展名。

