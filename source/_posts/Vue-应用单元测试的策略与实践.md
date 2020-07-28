---
title: Vue 应用单元测试的策略与实践
date: 2020-07-28 15:50:18
tags: 单元测试
---
## 单元测试基础

* 没有单元测试，开发者会倾向于不变动代码，导致代码腐化
* 加快开发速度
* 测试是重构的唯一保障
<!-- more -->
### 前端测试框架

#### 测试运行器

测试运行器 (test runner) 就是运行测试的程序。Vue Test Utils 是 Vue 官方提供的测试工具，可以支持主流的测试运行器。

* Jest 是一个“零配置”的 Facebook 开发的前端测试工具。它提供了一种“零配置”的开发体验，并具备诸多开箱即用的功能，比如 Mock 和代码覆盖率等。
* mocha-webpack 是一个 webpack + Mocha 的包裹器，同时包含了更顺畅的接口和侦听模式。这些设置的好处在于我们能够通过 webpack + `vue-loader` 得到完整的单文件组件支持，但这本身是需要很多配置的

#### 浏览器环境

JSDOM 可以在 Node 端启动一个虚拟的浏览器环境。

Jest 内置 JSDOM。

#### 单文件组件

Vue 的单文件组件在它们运行于 Node 或浏览器之前是需要预编译的。

* Jest 内置预编译器
* Webpack 编译

### Vue 官方介绍的三种方案

* Jest：零配置，内置JSDOM，内置预编译器，可以生成测试覆盖率报告，支持快照测试，最重要的是快

* Mocha + Webpack：不需要对源代码做妥协，使用 Chai 断言库，使用 Sinon 做替身，也可以使用 jest/except 语法

* Karma + Mocha ：使用 Chai 断言库

#### 选择 Jest 的理由

* 运行速度快
* 零配置，内置多种工具，内置 Mock（干掉 Sinon）、Test Runner（干掉 Karma）、Matcher（干掉 Chai）、Test Coverage（内置 istanbul）
* Watch Mode 守护模式。非常注重开发者体验，能够在编码的时候帮助我们快速获得测试结果的反馈
* 支持快照测试
* 由 Facebook 出品，文档和示例丰富，有大量的可参考文档

## Jest 单元测试基础

#### Given/When/Then

测试主体最小单元，采用了 Given When Then 的经典格式，称为测试三部曲

* Given：准备测试需要的数据

* When：调用相应的模块执行对应的方法或函数

* Then：断言

### 模块间依赖

使用替身来代替被依赖的组件、网络请求、IO读取、数据库、外部系统

* Mock 用来替代整个模块
* Stub 用来模拟特定行为
* Spy 用于监听模块行为

### 如何测试异步代码

* Callback 回掉函数
* Promise
* Async/Await

## Vue组件单元测试

## Vue 组件树的测试

在单元测试中，通常我们希望将重点放在作为独立单元进行测试的组件上，并避免间接断言其子组件的行为

针对子组件，足够小的单元进行完整详尽的测试，在组件树的层级中越高，测试颗粒越粗

### 浅渲染

```javascript
shallowMount(component[, options]) => Wrapper
```

针对某个上层组件进行测试时，可以不用渲染它的子组件，所以就不用再担心子组件的表现和行为，这样就可以只对特定组件的逻辑及其渲染输出进行测试

### 全量渲染

```javascript
mount(component[, options]) => Wrapper
```

会将 Vue 组件和所有子组件渲染为真实的 DOM 节点

### 静态渲染

```javascript
render(component[, options]) => CheerioWrapper
```

会将 Vue 组件渲染成静态的 HTML 字符串，而返回的则是一个 Cheerio 实例对象，采用的是一个第三方的 HTML 解析库 Cheerio，这是一个类 jQuery 的库，可以在 Node.js 中遍历 DOM

### 纯字符串渲染

```javascript
renderToString(component[, options]) => string
```

把一个组件渲染成对应的 HTML 字符串

### UI 组件交互行为的测试

通过 click 或者 trigger 来传递数据进去，等待事件返回

##Vuex 单元测试

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

#### mutations 测试

架构简单、逻辑简单的 mutation 不需要测试。

在保存数据的同时还进行去重、合并、排序这类复杂操作的 mutation 具备测试价值。

#### actions 测试

当测试 actions 的时候，需要用到 Mocking 服务层来模拟 action 可能调用的外部 API。

#### getters 测试

getter 的测试与 mutation 一样直截了当。仅需判断输入输出。

### Vue 组件 和 Vuex store 的交互

在单元测试的时候，shallowMount（浅渲染）方法接受一个挂载 options，可以用来给 Vue 组件传递一个伪造的 store。所以我们在测试 action 的时候就可以只关心 action 的触发，而至于触发之后对 store 做了什么事情我们就不需要再关心了，因为 Vuex 的单元测试会涵盖相关的代码逻辑。

Vuex 等 `Redux-like` 架构在前端应用中的 “状态管理模式” ，已经将 View 视图层和 State 数据层尽可能合理得拆分与隔离，那么单元测试就只需要分别测试 Vue 和 Vuex，从而就能保证 Vue 组件和数据流按照预期那样工作。

## 测试策略

### 单元测试的特点及其位置

单元测试写起来相对最容易、运行速度最快、反馈效果又最直接。

自动化测试技术进行分层

* **使用静态类型系统和 linter** 来捕获拼写或语法之类的基本错误
* **编写有效单元测试** 需要特别针对于应用的某些关键行为或功能
* **编写集成测试** 以确保 Web 应用各模块之间能够正常协调工作
* **创建端到端（e2e）功能测试** 对关键路径进行自动化点击操作，而不是等到最终用户来发现问题

因为单元测试**成本相对最低，运行速度最快**（通常是毫秒级别），而对单元的保护价值相对更大，所以在自动化测试中是性价比最高的一个环节。

### Vue 应用测试的测试策略

针对 Vue 应用的代码层级来进行策略性的单元测试

| 架构层级  | 测试内容                                                     | 测试策略                           | 说明                                                         |
| --------- | ------------------------------------------------------------ | ---------------------------------- | ------------------------------------------------------------ |
| action    | 是否获取了正确的参数<br />是否正确的调用了 API<br />是否使用了正确的返回值存取回 Vuex 中<br />业务分支逻辑<br />异常逻辑 | 5个点应该全覆盖                    | 这5个点覆盖大部分业务逻辑，具有很大价值                      |
| mutation  | 是否正确完成计算                                             | 全覆盖                             | 这个层级输入输出明确，又包含业务计算，非常适合单元测试       |
| getter    | 是否正确完成计算                                             | 全覆盖                             | 这个层级输入输出明确，又包含业务计算，非常适合单元测试       |
| component | 是否渲染了正确的组件                                         | 测分支渲染逻辑<br />测点击交互逻辑 | 这个层级最为复杂，还是以「代价最低，收益最高」为指导原则进行 |
| UI        | 组件是否渲染了正确的样式                                     | 纯 UI、CSS 不测                    | 这个层级成本最高                                             |
| utils     | 各种辅助工具函数                                             | 全覆盖                             |                                                              |

#### Component 的测试策略

Vue 组件是一个高度自治的单元，从分类上来看，它大概有这么几类：

- 展示型业务组件
- 容器型业务组件
- 通用 UI 组件
- 功能型组件

Vue 组件描述了页面的 UI 内容、结构、样式和一些逻辑 `component(props) => UI`。内容、结构和样式，比起测试，直接在页面上调试反馈效果更好。逻辑这块，有一测的价值，但需要控制好依赖。

所以不管哪种类型组件，有两个点一定要测

* 组件分支渲染逻辑
* 事件调用和参数传递

#### 单元测试的 F.I.R.S.T 原则

- F Fast：测试需要频繁运行，因此要能快速运行；
- I Independent：测试应该相互独立，一次只测一条分支；
- R Repeatable：测试本身不包含逻辑，能在任何环境中重复；
- S Self-validating：只关注输入输出，不关注内部实现；
- T Timely：测试应该及时编写，表达力极强，易于阅读；

### 参考

* [Vue test utils ]: https://vue-test-utils.vuejs.org/zh/

* [Vuex]: https://vuex.vuejs.org/zh/guide/testing.html

* [Jest]: https://jestjs.io/docs/en/getting-started

* [Vue 应用单元测试的策略与实践]: https://blog.jimmylv.info/2018-09-19-vue-application-unit-test-strategy-and-practice-01-introduction/

* [一次搞懂單元測試、整合測試、端對端測試之間的差異]: https://blog.miniasp.com/post/2019/02/18/Unit-testing-Integration-testing-e2e-testing/

* [Test Properties and Custom Events in Vue.js Components with Jest]: https://alexjover.com/blog/test-properties-and-custom-events-in-vue-js-components-with-jest/

* [Vue 测试指南]: https://lmiller1990.github.io/vue-testing-handbook/zh-CN/

* [Unit testing Vuex actions with Jest mocks]: https://codeburst.io/a-pattern-for-mocking-and-unit-testing-vuex-actions-8f6672bdb255

