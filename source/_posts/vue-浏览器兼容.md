---
title: vue 浏览器兼容
date: 2020-07-14 11:08:40
tags:
   - Vue
---
### browserslist

在`package.json`中有一个`browserslis`配置项是用来配置该工程的目标浏览器范围。

使用`@vue/cli`初始化的工程该配置项默认为`['> 1%','last 2 versions']`,在工程路径下执行`npx browserslist`可以列出该配置支持的浏览器列表。

同时这个值会被 [@babel/preset-env](https://new.babeljs.io/docs/en/next/babel-preset-env.html) 和 [Autoprefixer](https://github.com/postcss/autoprefixer) 用来确定需要转译的 JavaScript 特性和需要添加的 CSS 浏览器前缀。

### polyfill

`polyfill`可以理解为补丁，现代 es6 语法在不支持的浏览器上可以通过补丁代运行，比如 `=>`箭头函数在 ie 上无法运行，polyfill可以把箭头函数转移为 es5 的语法实现。

### 什么时候需要用到 polyfill

很不幸，如果你是一个始终使用最新`ECMAScript`的开发者，你的项目离不开 polyfill。合理配置 polyfill 可以保持开发时使用最新最爽最便捷的语法还能兼容不同版本浏览器的运行。

### 那些代码需要使用 polyfill

有两部分代码需要使用 polyfill 来转移，分别是 `/src` 下的源码还有`node_modules`下的依赖。

#### src

`/src`下是工程里我们存放源码的地方，这里的代码主要是开发者所写，编写时很容易使用新语法，所以需要转译。

#### node_modules

`/node_modules`是工程的依赖，很不幸我们不能指定依赖采用什么语言，如果最终编译后源码转译而依赖没有转译一样会遇到兼容性问题。

### 如何使用 polyfill

#### babel.config.js

使用 Vue Cli 创建的工程会有一个 `babel.config.js` 文件用来配置需要转译的范围，默认为`useBuiltIns:'usage'`，polyfill 会根据 browserslist 自动检测源码中需要转译的部分，确保最终包里 polyfill 数量的最小化。

#### vue.config.js

如果有依赖需要 polyfill ，则需要在 `vue.config.js` 中增加`transpileDependencies`项指定哪些依赖需要被转译。

大多数情况下使用的依赖过多，则可以设置为`['*']`，这样所有的依赖最终都会转译。

### 避免麻烦的最好方法

避免麻烦的最好方法往往是简单粗暴但却最不优雅的。

* 在`babel.config.js`中改`usage`为`entry`
* 同时在`main.js`入口位置增加`import 'core-js/stabl'` 和 `import 'regenerator-runtime/runtime'`（如果提示没有安装`core-js`，执行安装 `npm i core-js@3`）
* 在`vue.config.js`中设置`transpileDeoendencies`为`['*']`

这样做之后，所有的源码和依赖都会根据browserslist中的目标浏览器范围被polyfill转译为可执行的 es5 代码。

缺点就是开发时编译会慢，最终包会更大一些。
