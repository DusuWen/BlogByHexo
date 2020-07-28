---
title: 使用vuex和immutable实现undo和redo
date: 2019-06-03 15:02:56
tags:
---

## 使用Vuex实现undo

在最近的一个项目里需要在一个基于canvas进行标注的页面实现用户的撤销操作，刚开始查资料，有介绍可以使用immutable.js来实现。

### 什么是immutable.js

[immutable.js](https://immutable-js.github.io/immutable-js/)就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是 **Persistent Data Structure（持久化数据结构）**，也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。
<!-- more -->
## 设计过程

看介绍，immutable很厉害，但是这个是与React搭配可以发挥最大价值的武器，在网上搜索一番，也没有找到配合Vuex使用的例子。所以，最后实现undo还是只能采用普通的array方法。

简单一点就是，每一次标注动作完成后程序都会执行`push`操作，把标注动作最后的状态`push`到状态快照数组内，而执行`undo`其实就是取出`pop`出最后一次状态的数据，重新赋值给`canvas`绘制函数，当页面绘制出上一个状态时，也就完成了一次`undo`操作。

## 遇到的问题

当执行一次`undo`时，这次操作要不要再推送到状态快照数组内，这里实际上涉及到是否实现`redo`功能。
