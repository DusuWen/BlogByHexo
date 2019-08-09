---
title: '[译]我用React和Vue创建了一个同样的app，这里是他们的不同之处'
date: 2019-08-09 16:58:07
tags:
---

原文地址：[I created the exact same app in React and Vue. Here are the differences.](https://medium.com/javascript-in-plain-english/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-e9a1ae8077fd)

工作中在用Vue，所以我对它有很扎实的理解。我以前就很好奇，篱笆另一边的草是什么样子的，这里说的草就是React。

我读过React的文档，也看过一些教学视频。他们是不错，但是我真的想知道的是React和Vue到底有多不同。这里说的‘不同’，不是说他们都在用虚拟Dom或者他们渲染页面的方式。我以为会有一个人愿意花点时间去解释代码层面的不同，所以我没少花时间去找这么一篇描写React和Vue哪里不同的文章，好让我对他们直接的不同之处有更深入的理解。

不幸的是，我没找到。所以我意识到只有自己动手才能看到他们的相同与不同。在这种情况下，我觉得应该把这个过程记录下来，这也就是这篇文章最后诞生的原因。

![Who wore it better?](https://miro.medium.com/max/700/1*WRzDZndJCduHwqgOpWmbhQ.png)

我决定去创建一个相同标准的To-Do App可以让用户向列表中添加或者删除条目。两个App都是使用了默认的脚手架工具（React用create-react-app，Vue用vue-cli）。顺便一提，脚手架工具用命令行。

**行吧，这段导言已经写的比我预料的还要长。让我们开始看一下这两个app长什么样子吧。**

![Vue vs React: The Irresistible Force meets The Immovable Object](https://miro.medium.com/max/1266/1*mJ-qdNqldpgae2U5oS0qDg.png)

两个App的CSS部分一模一样，但是他们存放的位置不同，有鉴于此，让我们看一下两个App的文件结构：

![](https://miro.medium.com/max/700/1*rahCwWEIXM7Wblk4L9ExYA.png)