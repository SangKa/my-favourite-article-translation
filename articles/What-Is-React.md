# [译] 图解 React

> 原文链接: [https://learnreact.design/2017/06/08/what-is-react](https://learnreact.design/2017/06/08/what-is-react)
>
> 喜欢理由: 插图大爱 生动有趣 视角独到

系列博客: 用通俗的语言和涂鸦来解释 React 术语

  * 图解 React (本文)
  * [图解 React Native](./What-Is-React-Native.md)
  * [组件、Props 和 State](./Components-Props-State.md)
  * [深入理解 Props 和 State](./Props-And-State-Re-explained.md)
  * [React Native vs. Cordova、PhoneGap、Ionic，等等](./React-Native-VS-Cordova-PhoneGap-Ionic-Etc.md)

React、ReactJS、React.js、React Native… 这些有些相似的名词你最近听过多少遍了？对于它们究竟是什么你是否感到困惑？

如果你是一名设计师，你所在的团队使用(或正在考虑使用)的技术是 React ，或者你只是单纯对 “React” 比较好奇的话，那么本文就是为你而准备的。

在文本中，我只使用朴实的语言和插图来解释 React 家族中的各种术语，并深入探索究竟是什么使得 React 如此特别。本文中并不需要任何代码知识便可阅读。我希望你先熟悉一些概念，从而不至于在后面的学习过程中感到绝望。如果后面需要温故而知新的话，欢迎随时回来阅读。

准备好了吗？我们开始了！

## 学习目标

读完本文后，希望你能够重新回到这里，并能够轻松回答下列问题:

  * 什么是 DOM ？
  * 什么是 React ？它的哪些方面比较适合应用开发？
  * React 与 jQuery 的不同之处？
  * React 的核心概念是什么？
  * 什么是响应式 UI ？
  * 组件有哪些好处？

## 关于 Web 你需要了解的

我们先来介绍一些你可能听过很多年的术语。首先是 DOM 。

### DOM

DOM 的全称是 Document Object Model (文档对象模型)。很简单吧？它就是文档对应的对象模型。

先暂时忘掉它的概念。我们先来看看大名鼎鼎的 “Web Browser” 工作室！你能在下面的插图中找到 DOM 吗？

![browser](../assets/What-Is-React/browser.png)

难道 DOM 是……一棵树？对，就是一棵树！奇怪的是，计算机相关的很多东西其实都像是一棵树。

我们来给 DOM 起个昵称……就叫 Domo 如何？Domo 是 “Web Browser” 工作室的御用模特，他的工作就是在肖像画家(也可能是数百万个画家)面前摆 pose 。

肖像就是在浏览器中浏览网站时所看见的内容。开发者的职责就好比是导演，他来告诉 Domo 该穿什么衣服，摆什么 pose 。这将决定肖像最终画出来的样子。jQuery 和 React 都是库，开发者使用它们作为与 Domo 交流的工具。

### jQuery

jQuery 是一个 JavaScript 库，它可以使开发者操纵 DOM 变得简单得多。那他在 Domo 的故事中又扮演什么角色呢？

它是一个工具，可以简化开发者与 Domo 沟通的过程，就像是一部手机。无论何时何地都可以轻松呼叫 Domo 。相比于之前(使用原生 JavaScript)，它要方便得多，还记得在电话发明出来之前人跟人连简单交流都要走得足够近才行。

![jquery](../assets/What-Is-React/jquery.png)

多年以来，我们一直都在使用 jQuery 来直接与 Domo 沟通。是很方便，但并非没有问题。

## React

下面请允许我来为你介绍一个全新的超级英雄: React 。

![react](../assets/What-Is-React/react.png)

使用 React 的话，开发者不再需要直接跟 Domo 沟通。React 扮演在开发者和 Domo 之间的中间人角色。他降低了两者之间的沟通成本，同时简化了肖像创建的过程。

![middleman](../assets/What-Is-React/middleman.png)

React 使用了一些技术来解决 jQuery 和其他工具中所存在的问题。下面是它的三项核心技术:

  * 响应式 UI
  * 虚拟 DOM
  * 组件

### 响应式 UI

使用 jQuery 来更新 DOM 的话，你需要在适当的时机以正确的顺序来指定要更改的元素。这等同于给 Domo 一步步讲述头怎么摆、胳膊放在哪、腿什么姿势，等等，并且每张肖像都是如此。

![step-by-step](../assets/What-Is-React/step-by-step.png)

我靠，这听起来太乏味了，并且容易出错！为什么不直接告诉 Domo 你想要的效果，而不是现在这样一步步地告诉他怎么摆 pose ?

![thinker](../assets/What-Is-React/thinker.png)

还有更酷的，想象一下如果可以在要求过程中保留一个占位符来表示相同姿势的不同变体。React 就能做到！

这种方式的话，当画家要求 Domo 穿戴不用的帽子作画时，你不需要每次都告诉 Domo 戴哪顶帽子。你尽管坐在一旁让他自己换帽子即可。

![thinker-with-hat](../assets/What-Is-React/thinker-with-hat.png)

这项技术正是 React 名字的由来。使用 React 构建的 UI 是**响应式**的。作为开发者，你只需编写你想要的是**什么**，React 自己会弄清楚该**怎么**做。当数据变化时，UI 会相应地发生改变。你无需再关心 DOM 的更新，React 会自动帮你完成。响应式 UI 的理念大大地简化了 UI 开发。

我知道我说过你不需要任何编码知识，但只是为了帮助你正确地看待问题，我还是用代码把它写了出来。请查看下面的示例(尝试更换 Domo 的帽子)):

Codepen 在线 Demo: [Domo 的帽子](https://codepen.io/focuser/pen/gROrXx) 。

在后面的文章中我会来讲解完整的代码，但此时你只需简单看一眼关键代码即可:

```js
const ThinkerWithHat = ({ hat }) => (
  <div>
    <Hat type={hat} />
    <Thinker />
  </div>
);
```

注意，你只需定义你想要的 (戴帽子的思想者)，并“连接”上数据 (`“type = {hat}”`) 。当数据发生变化时 (用户选择一顶帽子)，UI 会自动更新。

### 虚拟 DOM

jQuery 的另一个问题就是它的运行速度。

作为一个严苛的导演，你讨厌等待。你想要肖像画尽可能快地完成。但是，Domo 和画家都比较慢，并非是树濑那种慢，只是 Domo 需要时间来换装和摆 pose ，并且画家作画也需要时间。

更糟糕的是，在画家完成一幅肖像画之前，你无法与 Domo 进行沟通。事实上，你什么也做不了，除了等待。真浪费时间！

![slow-dom](../assets/What-Is-React/slow-dom.png)

React 采用了另一项技术来解决此问题。React 画草稿的速度超级快。是当你告诉他你的要求后，他几乎就能立即将草稿完成并准备画下一张。现在就无需等待了！你可以不停地告诉 React 你想的肖像。React 将会纪录草稿的所有细节，并在适当的时候展示给 Domo 看。

![sketches](../assets/What-Is-React/sketches.png)

更重要的一点是 React 十分聪明。他还会对所有草稿进行整理，拿掉重复的并确保 Domo 和画家的工作量维持在最低水平。

![optimize-sketches](../assets/What-Is-React/optimize-sketches.png)

这些草稿就是 “虚拟 DOM” 。虚拟 DOM 要比操纵 DOM 快得多得多。开发者绝大部分时间里其实都是在操纵虚拟 DOM ，而不是直接操纵真实的 DOM 。React 负责管理 DOM 的这部分脏活。

### 组件

React 中第三项技术就是组件的概念。

组件应该很容易理解，因为我们所生活的现实世界就是由组件组成的。我们的车、房，甚至是身体都是由不同的组件所组合而成的。这些组件又是由一些更小的组件组合而成，以此类推，直至分解成原子。

如果你熟悉 [Sketch](https://www.sketchapp.com/) (译者注: 著名的设计软件，与 PhotoShop 齐名) 的话，组件与 Sketch 中的 [symbols](https://www.sketchapp.com/docs/symbols/) 十分类似。构建 React 应用几乎都是在同组件打交道: 寻找最适合的组件、融合两个组件、在现有组件的基础上创建新组件，等等。

回到 “Web Browser” 工作室，你将肖像的需求描述成一个个组件，React 将这些组件翻译成 Domo 所能理解的内容。这将为你节省大量时间，因为你无需再一次次地重复描述需求中的通用部分。

![components](../assets/What-Is-React/components.png)

组件另外很酷的一点是如果你改变了某个组件，那么所有使用此组件的地方都将自动更新。

![hair-style](../assets/What-Is-React/hair-style.png)

## 总结

好了。希望你能学会一些 React 的知识。本质上，它还是一个工具，用来帮助开发者操纵 DOM ，从而构建出页面。响应式 UI 、虚拟 DOM 和组件是 React 的三大核心概念，正是有了它们才使得 React 如此特别。当然，React 还有一些其他有趣的概念，比如单向数据流，我会在后面的文章中介绍。

在[下一篇文章](https://learnreact.design/2017/06/20/what-is-react-native/)中，我们将介绍 ReactJS、React Native 和 React Sketch.app 之间的关联和区别。

我鼓励你回到[学习目标](#学习目标)那里，去试试自己是否能够回答出全部问题。如果你有任何问题或意见，请给我留言!
