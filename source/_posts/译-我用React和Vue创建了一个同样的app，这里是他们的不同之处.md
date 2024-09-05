---
title: '[译]我用React和Vue创建了一个同样的app，这里是他们的不同之处'
date: 2019-08-09 16:58:07
tags:
  - 译
---

原文地址：[I created the exact same app in React and Vue. Here are the differences.](https://medium.com/javascript-in-plain-english/i-created-the-exact-same-app-in-react-and-vue-here-are-the-differences-e9a1ae8077fd)

工作中在用Vue，所以我对它有很扎实的理解。我以前就很好奇，篱笆另一边的草是什么样子的，这里说的草就是React。

我读过React的文档，也看过一些教学视频。他们是不错，但是我真的想知道的是React和Vue到底有多不同。这里说的‘不同’，不是说他们都在用虚拟Dom或者他们渲染页面的方式。我以为会有一个人愿意花点时间去解释代码层面的不同，所以我没少花时间去找这么一篇描写React和Vue哪里不同的文章，好让我对他们直接的不同之处有更深入的理解。
<!-- more -->
不幸的是，我没找到。所以我意识到只有自己动手才能看到他们的相同与不同。在这种情况下，我觉得应该把这个过程记录下来，这也就是这篇文章最后诞生的原因。

![Who wore it better?](https://miro.medium.com/max/700/1*WRzDZndJCduHwqgOpWmbhQ.png)

我决定去创建一个相同标准的To-Do App可以让用户向列表中添加或者删除条目。两个App都是使用了默认的脚手架工具（React用create-react-app，Vue用vue-cli）。顺便一提，脚手架工具用命令行。

**行吧，这段导言已经写的比我预料的还要长。让我们开始看一下这两个app长什么样子吧。**

![Vue vs React: The Irresistible Force meets The Immovable Object](https://miro.medium.com/max/1266/1*mJ-qdNqldpgae2U5oS0qDg.png)

两个App的CSS部分一模一样，但是他们存放的位置不同，有鉴于此，让我们看一下两个App的文件结构：

![](https://miro.medium.com/max/700/1*rahCwWEIXM7Wblk4L9ExYA.png)

你可以看到，他们的文件结构几乎就是相同的。唯一的不同是React有三个css文件，Vue没有。

原因是，在create-react-app中，一个React组件总是一个跟着它的文件保存着样式，而Vue CLI采用内含的方式，把声明的样式放在实际组件内部。

最终，这两种不同的方式都起到了同样的效果，而且进一步的说，你也不需要在React和Vue中构造不同的CSS。这真的取决于个人喜好，你会听到很多来自开发社区关于如何组织CSS文件的讨论。现在，我们就遵守各脚手架工具默认的结构了。

在我们进一步深入之前，先让我们看一下典型的React和Vue组件长什么样子。

![](https://miro.medium.com/max/1000/1*yQS8va-QXM2poiP-RqasOw.png)

#### 我们如何变更数据

首先，“变更数据”到底是什么意思？听起来有点技术含量，对不对？大概就是说把我们已经保存的数据给改变了。所以如果我们想把一个人的名字从John改成Mark，我们就是在变更数据。所以，这就是React和Vue不同点的关键所在。当Vue实际上创建一个数据对象（data object），意味着数据可以自由更新，但是React创建一个状态对象（state object），需要一点额外的工作，把对象带出去再更新。但是React采用这种需要一点额外的工作的方式有自身的理由，稍后我们再深入了解。首先，让我们看一下数据对象（data object）和状态对象（state object）的不同：

![](https://miro.medium.com/max/301/1*b9BjPHgneHv2K6ZYlAoe8A.png)

![](https://miro.medium.com/max/292/1*asy_vlGoZgtA3sAA7Dw4CA.png)

所以你可以看到我们给两个组件赋予了同样的数据，但是他们只是简单地标记不同。所以，最开始传递数据是非常非常相似的。但正如我们已经提到过的，我们如何在两个框架里改变数据是不同的。

我们一起看一下，有一个数据叫做name：‘Sunil’。

在Vue中，我们通过使用this.name来引用这个数据。我们也可以通过this.name = 'John'来更细数据。这样做会把我的名字改成'John'，我不知道别人叫我John是一种什么样的体验，但是你看，改成功了。

在React中，我们通过使用this.state.name来引用相同的数据。现在有一个关键的不同之处在于我们不能简单的写this.state.name = 'John'，因为React对阻止这类简单、清晰的数据变更做了一些限制。所以在React中，我们需要单独写一行this.setState({name:'John'})。

其实这跟我们在Vue中做的事情是一样的，需要单独多写一行是因为当数据更新时Vue本质上默认把setState结合到框架里了。简而言之，React需要在setState中更新数据，而Vue认为你在数据中进行更新是因为你想更新。为什么React多此一举呢，为什么需要用setState呢？我们可以看一下[Revanth Kumar](https://medium.com/@revanth0212)这么解释的：

> 这是因为当数据变更时React想重跑整个生命周期钩子，例如componentWillReceiveProps，shouldComponentUpdate, componentWillUpdate, render, componentDidUpdate。在你使用SetState后React会知道state发生了变化。如果你直接变更数据，React需要做更多额外的工作来保持数据变更轨迹和生命周期这些。所以为了React简单点，就用了setState。

现在我们已经讲过变更数据了，让我们深入事情本质来看看如何在两个To Do App里增加一个新的项。

#### 我们如何创建一个To Do 项？

React：

```javascript
createNewToDoItem = () => {
    **this**.setState( ({ list, todo }) => ({
      list: [
          ...list,
        {
          todo
        }
      ],
      todo: ''
    })
  );
};
```

##### React怎么做的？

在React中，input标签上有一个叫做value的属性。这个value会通过几个捆绑在一起的很像Vue‘双向绑定’（如果你对双向绑定这个词不太熟悉，稍后的文章中会有更细节的解释）的函数得到自动更新。我们创建input标签的时候还附加了一个叫做onChange的事件监听器。我们简单看一下这个input标签，你就明白是怎么回事了：

```javascript
<input type="text" 
       value={this.state.todo} 
       onChange={this.handleInput}/>
```

一旦input标签中的值发生了变化，处理变化的函数（handleInput）就会执行。这个函数会把输入input里的值赋值给state中的todo 对象。这个函数长这样：

```javascript
handleInput = e => {
  this.setState({
    todo: e.target.value
  });
};
```

现在，每当用户按下页面上的+按键增加todo项时，createNewToDoItem函数实际上是运行了this.setState并给它传递了一个函数。这个函数包含两个参数，第一个是赖在state对象里的整个list数组，第二个是todo项（由handleInput函数传递过来）。这个函数会返回一个新对象，这个对象是之前的list数组，并在todo项在这个数组末尾。向这个数组添加项使用到了展开语法（如果你不熟悉的话，百度一下ES6 语法）。

最后，我们向todo里传入了一个空字符串，这也会自动更新input标签中的value属性。

Vue：

```javascript
createNewToDoItem() {
    this.list.push(
        {
            'todo': this.todo
        }
    );
    this.todo = '';
}
```

##### Vue是怎么做的呢？

在Vue中，input有一个处理器叫做v-model。它允许我们可以做到的所谓“双向绑定”。让我们先看一下代码，然后再解释发生了什么。

```javascript
<input type="text" v-model="todo"/>
```

v-model通过key来绑定input输入框和data 对象中的toDoItem。在页面加载之后，我们会把toDoItem设置为空字符串，例如todo：‘’。如果todo已经有值，例如todo：‘add some text here’，我们的input输入框也会有“add some text here”。回到我们一开始设置todo为空字符串的时候，不管我们在input框输入什么，他都会绑定到todo上。这就是我们所说的双向绑定（input输入框可以试data对象更新，data对象可以试input更新）。

所以回头看之前的createNewToDoItem（）代码块，我们可以看到，我们把todo中的文本push（）到list数组，只不过todo里是一个空字符串。

#### 我们如何从列表中删除数据？

React：

```javascript
deleteItem = indexToDelete => {
    this.setState(({ list }) => ({
      list: list.filter((toDo, index) => index !== indexToDelete)
    }));
};
```

##### React怎么做的？

