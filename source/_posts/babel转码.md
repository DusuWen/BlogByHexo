---
title: Babel 转码
date: 2020-11-17 10:13:26
tags:
---

Babel 是如何把 ES6 转码成 ES5 的

### 什么是 ES6、ES5

ECMAScript 6.0 简称 ES6 是下一代 JavaScript 语言标准，于 2015年发布，所以又称为 ES2015。ECMAScript 和 JavaScript 的关系是，前者是后者的规格，后者是前者的一种实现。

<!-- more -->

2011年 ECMAScript 5.1标准发布后，下一代标准就是6.0，2015年 ECMAScript 6 标准发布，此后每年6月 ECMA 委员会都会批准通过一些新的特性和语法，对应的就是 ES6.1，又因为每年6月发布标准，所以每年发布的增订版 ES6又会根据年份称为 ES2016，ES2017，ES2018，直到现在的ES2021。

ES6 是一个泛指 ES5之后的新版本标准，而每年的例如 ES2015 则是正式的名称。

### 什么是 Babel

[Babel](https://babeljs.io/) 是一个广泛使用的 ES6 转码器，可以将 ES6 代码转为 ES5 代码，从而在老版本的浏览器执行。这意味着，你可以用 ES6 的方式编写程序，又不用担心现有环境是否支持。通过 Babel 及其插件不仅可以转换新的语法还有新的 API。

### 为什么转码

根本原因是不同种类和版本的浏览器对 JavaScript 语言标准支持的程度不同，新版的 Chrome 可以使用的箭头函数在 IE 上不可以使用，这就要前端针对不同的浏览器开发不同的代码吗？不，我们可以通过转码工具来抹平不同种类、版本浏览器之间的差异。

### 转码原理

#### Parse 解析

第一步主要是将 ES6 语法解析为 AST 抽象语法树。简单地说就是将代码打散成颗粒组装的对象。这一步主要是通过 babylon 插件来完成。

#### Transformer 转换

第二步是将打散的 AST 语法通过配置好的 plugins（babel-traverse 对 AST 进行遍历转译）和 presets （es2015 / es2016 / es2017 / env / stage-0 / stage-4 其中 es20xx 表示转换成该年份批准的标准，env 是最新标准，stage-0 和 stage-4 是实验版）转换成新的 AST 语法。这一步主要是由 babel-transform 插件完成。plugins 和 presets 通常在 .babelrc 文件中配置。

#### Generator 生成

第三步是将新的 AST 语法树对象再生成浏览器都可以识别的 ES5 语法。这一步主要是由 babel-generator 插件完成。

### 如何使用

不管是用三大框架还是独立开发，都可以使用 babel 及其配套插件，按照文档根据需求配置转码实现的方式。
