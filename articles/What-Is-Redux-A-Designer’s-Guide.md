# [译] 从设计师的角度看 Redux

> 原文链接: [https://www.smashingmagazine.com/2018/07/redux-designers-guide/](https://www.smashingmagazine.com/2018/07/redux-designers-guide/)
>
> 喜欢理由: 插图大爱 抛开文字 光是这些精美的涂鸦就让我有阅读的欲望

#### **内容概要**: 你是否知道 Redux 的真正威力远不止状态管理吗？你是否想要了解 Redux 的工作原理？让我们来深入介绍 Redux 到底能做些什么？为什么它是这样设计的？它的缺点有哪些？以及它与设计有哪些关联？

#### **你是否听说过 Redux ？它到底是什么？不允许 Google !**

  * “花哨的后端技术。”
  * “我听说过，但不知道是干什么用的。好像是一个 React 框架？”
  * “是一种在React 应用中存储管理状态的更好方式。”

这个问题我曾问过不下于40个设计师。上面列出的是他们的经典回答。他们中不少人都知道 Redux 是和 React 一起工作的，并且它的职责是“状态管理”。

但是你可知道这个“状态管理”的真正含义吗？你是否知道 Redux 的真正威力远不止状态管理吗？你是否知道 **Redux 并非一定要搭配 React 来使用**？你是否想要加入团队谈论(至少是午餐讨论)，关于是否使用 Redux ？你是否想要了解 Redux 的工作原理？

本文的目的就是**让你对 Redux 有更全面的认知**: 它能做什么？为什么它要这样设计？何时使用它？以及它与设计有哪些关联？

我的目标是帮助像你一样的设计师。尽管你可能连一行代码都写过，不过我认为还是可以理解 Redux的，并能从中受益和享受乐趣。贯穿全文的只有朴实的语言及有趣的涂鸦，没有任何代码及高谈阔论。

准备好了吗？

## 什么是 Redux ？

从大局来看的话，Redux 是一种让开发者的工作更为轻松的工具。正如你所听过的，它的职责是“状态管理”。稍后我将会解释什么是状态管理。此刻，我只能想让你看下面这张图:

![redux-state-manage-power](../assets/What-Is-Redux-A-Designer’s-Guide/redux-state-manage-power.png)

:framed_picture: Redux 用来管理状态，但在状态管理的背后，还有一些隐藏的能力 ([Beebee](https://beebeeye.github.io/) 作图)

## 为什么要关心 Redux ？

与 Redux 相关的，更多的是应用的内部使用，而不是外观感受。它是一个复杂的工具，学习曲线很陡。这是否意味着作为设计师的我们应该对它避而远之呢?

不。我觉得我们应该拥抱它。汽车设计师应该理解引擎是做什么的，你说是吗？要想成功地设计应用的界面，**设计师应该清楚地知道应用背后的一切**。我们应该了解它能做什么，理解开发者为什么要使用它，以及知道它的优点和局限性。

  > “设计部仅仅是外观感受。设计关乎于工作原理。”
  >
  > — 史蒂夫·乔布斯

## Redux 可以做什么？

许多人在 React 应用中谁用 Redux 来管理状态。这是最常见的用法，Redux 解决了 React 使用过程中的一些痛点。

但是，很快你就会感受到 Redux 的威力远不止于此。我们首先来介绍到底什么才是状态管理。

### 状态管理

如果你不确定这个“状态”到底表示什么，那我们用个更通俗的术语“数据”来进行替代。**状态是随时间流逝而产生变化的数据**。状态决定了展示给用户的界面。

到底什么才是状态管理？通常来说，在一个应用中需要管理的数据分为三种:

  1. [获取并存储数据](#获取并存储数据)
  1. [将数据分配给 UI 元素](#将数据分配给-UI-元素)
  1. [改变数据](#改变数据)

比如我们要做一个 Dribbble 的作品页面。在作业页面上我们想要展示的数据有哪些？其中包括作者的头像照片、名称、动态 GIF 图片、点赞数量、评论，以及等等。

![redux-dribbble-page-data](../assets/What-Is-Redux-A-Designer’s-Guide/redux-dribbble-page-data.png)

:framed_picture: Dribbble 作品页面的数据

首先，我们需要从云端服务器拉取这些数据并将其保存起来。接下来需要实际显示数据。我们需要将数据拆分开，然后分配给与之对应的 UI 元素，这些 UI 元素正是我们在浏览器中实际所见的。例如，我们将头像照片的 URL 分配给 `img` 标签的 `src` 属性:

```html
<img src='https://url/to/profile_photo'>
```

最后，我们需要处理数据的变更。例如，如果用户为作品添加了一条评论或点赞，我们需要更新相对应的 HTML 元素。

在前端开发过程中，整合这三类状态是一项大工程，**React 对此有着不同程度的支持**。有些时候，React 内置的功能就能很好的完成任务。但随着应用变得愈发复杂，单单依靠 React 进行状态管理会变得如履薄冰。这正是许多人开始使用 Redux 的初衷。

#### 获取并存储数据

在 React 中，我们将 UI 拆分成组件。每个组件又可以拆分成更小的组件。(参见 [“图解 React”](https://learnreact.design/2017/06/08/what-is-react?utm_source=smashing_magzine&utm_campaign=redux&utm_content=middle))

![redux-dribbble-components](../assets/What-Is-Redux-A-Designer’s-Guide/redux-dribbble-components.gif)

:framed_picture: Dribbble 的作品页面拆分成组件

页面的结构是这样的，那么在渲染页面之前我们何时去获取数据呢？又将数据储存在何处呢？

**想象一下，每个组件里都住着一位大厨**。从服务器获取数据就好比是采购所需的所有原材料以准备佳肴。

简单方式就是在需要的时候才去获取数据并将其储存起来。这样就好比每个大厨都驱车前往郊外的农场来采购蔬菜和肉类。

![redux-each-chef-buy](../assets/What-Is-Redux-A-Designer’s-Guide/redux-each-chef-buy.png)

:framed_picture: 简单方式: 每个组件各自获取自己所需要的数据 ([Beebee](https://beebeeye.github.io/) 作图)

这种方式是一种浪费。有多少个组件我们就得请求服务器多少次，即使是相同的数据。大厨们会浪费大量的汽油和时间在往返的路上。

使用 Redux ，我们只获取数据一次，并将数据存储在一个中心区域，通常称之为 “store” 。这样数据对于任何组件来说都可以随时使用。这就像附近有一家超市，我们的大厨们可以在那里买到所有食材。超市会派卡车去农场大批量地运回蔬菜和肉类。这比每个大厨都亲自去农场采购要有效率得多!

store 还是唯一的数据源。组件通常从 store 中获取数据，而不是其他地方。这使得 UI 保持高度统一。

![redux-redux-store](../assets/What-Is-Redux-A-Designer’s-Guide/redux-redux-store.png)

:framed_picture: Redux 将数据集中地存储起来 ([Beebee](https://beebeeye.github.io/) 作图)

#### 将数据分配给 UI 元素

如果单单使用 React 的话，实际上有一种更好的方式来获取并存储数据。我们可以请求非常善良的大厨 Shotwell 来为所有的厨师朋友们采购。他可以驱车前往农场将货物全部运回来。在 React 的世界中就是从一个容器型组件来获取数据，例如，Dribbble 示例中的 “Shot” 组件就是容器型组件，可以使用它来作为单一数据源。

![redux-driver-shotwell](../assets/What-Is-Redux-A-Designer’s-Guide/redux-driver-shotwell.png)

:framed_picture: 从根组件获取数据 ([Beebee](https://beebeeye.github.io/) 作图)

这种方式要比每个组件单独去获取数据要高效得多。但是大厨 Shotwell 如何将食材分给其他大厨们呢？换句话说，如何将数据传递给实际负责渲染 HTML 元素的组件？将数据从外层组件传递给内层组件就好比是接力赛中的接力棒，一层层地传递下去直到抵达目的地。

举个例子，作者头像的 URL 需要从 “Shot” 传出，然后传到 “ShotDetail”，再到 “Title” ，最后才能传给 `<img>` 标签。如果每个大厨都住在公寓里的话，应该就如下图中展示的一般:

![redux-floor-without-redux](../assets/What-Is-Redux-A-Designer’s-Guide/redux-floor-without-redux.png)

:framed_picture: 通过 props 将数据传递给目标组件 ([Beebee](https://beebeeye.github.io/) 作图)

要将数据传递给目标组件，我们需要使用传递路径上的所有组件，无论这些组件是否需要使用此数据。如果这个公寓是一个摩天大厦，那就太烦躁了！

如果超市送货上门呢？使用 Redux 的话，我们可以将任意数据提取至任意组件而压根不会影响到其他组件，就像这样:

  > 更准确地说，实际上是另一个叫做 `react-redux` 的库将数据提供给组件的，而并非 Redux 本身。但因为 react-redux 本身只是个连接库，并且开发者通常一起使用 Redux 和 react-redux ，因此我认为将它当做是 Redux 的好处之一是并无不妥。

![redux-floor-with-redux](../assets/What-Is-Redux-A-Designer’s-Guide/redux-floor-with-redux.png)

:framed_picture: 使用 Redux 将数据直接提取至目标组件 ([Beebee](https://beebeeye.github.io/) 作图)

**注意**: 在 React 的16.3版本中，提供了一个新的 “context” API ，它的提取数据功能几乎与 Redux 是相同的。如果你的团队使用 Redux 只为提取数据的话，不妨认真考虑将 React 版本升至16.3！想了解更多详情，请参见 [官方文档](https://reactjs.org/docs/context.html) (温馨提示: 文档中有大量代码) 。

#### 改变数据

有时候，在应用中更新数据的逻辑可能会相当复杂。它可能涉及到多个相互依赖的步骤。在更新应用的状态之前，我们可能需要等待多个服务器的响应。我们还可能需要根据不同条件、在多个事件点更新状态内的多处数据。

如果我们没有一个好的结构来实现所有这些逻辑，那将是毁灭性的，代码将难以理解与维护。

**Redux 可以让我们进行分治**。它提供了一种标准方式来将数据更新逻辑拆分成众多小块的 “reducers” 。这些 reducers 可以在一起协调工作，以完成复杂的动作。

![redux-from-spaghetti-to-reducer](../assets/What-Is-Redux-A-Designer’s-Guide/redux-from-spaghetti-to-reducer.png)

:framed_picture: 将复杂逻辑拆分成 reducer ([Beebee](https://beebeeye.github.io/) 作图)

没事可以多关注一下 React 的开发进展。就像 “context” API ，在 React 未来的版本中还可能会出现一个新的 “setState” API 。它可以将目前复杂的更新逻辑拆分成一个个小块。一旦这个新的 API 推出的话，很可能届时将不再需要 Redux 来管理状态。

### Redux 的真正威力

到目前为止，Redux 看上去只是 React 的辅助工具。开发者使用它来解决 React 的某些痛点。但 React 正在快速着手解决这些问题！事实上，Redux 的作者 Dan Abramov 在几年前已经加盟 Facebook 的 React 核心团队。他们一直致力于提升 React 的开发体验: context API (16.3版本发布)、更好的数据获取 API (详情请见 Dan Abramov 于2018年2月的[演讲](https://www.youtube.com/watch?v=v6iR3Zk4oDY))、更好的 setState API，等等。

这是否意味着 Redux 将被淘汰？

你猜呢？**我还未向你展示 Redux 的真正威力呢！**

![redux-other-powers](../assets/What-Is-Redux-A-Designer’s-Guide/redux-other-powers.png)

:framed_picture: Redux 的威力远不止状态管理 ([Beebee](https://beebeeye.github.io/) 作图)

Redux 强制开发者遵循几个原则，正是这些原则为 Redux 带来了强大的功能(这正是约束的力量！):

  1. 所有数据(应用的状态)都必须能够以文本的形式进行描述。你需要达到这种程度，用笔在纸上将所有的数据写出来。(译者注: 这里的数据应该指数据结构和数据类型两方面)
  1. 每个动作(改变数据的操作)都必须能够以文本的形式进行描述。你必须在改变数据之前将其写出来。没有动作就无法改变数据。在 Redux 的术语中这称之为 “派发 (dispatching) 动作”。
  1. 改变数据的代码必须能够像数学公式一样运行。给定相同的输入，必须返回同样的结果。无论计算多少次，4 的平方永远都是 16 。

当你遵循上述原则来开发应用的话，不可思议的事情就来了。Redux 将开启许多很酷的特性，这些特性使用其他技术很难实现，或者实现起来成本很高。下面是一些例子。

  > 我从 Dan Abramov 文章 [“You Might Not Need Redux”](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367) 和 [“React Beginner Question Thread.”](https://dev.to/dan_abramov/react-beginner-question-thread--1i5e/comments/1n21) 中收集了一些示例。

### 撤消、重做

流行的撤消/重做功能需要系统级的规划。因为撤消/重做需要记录并回放应用中发生的每次数据变化，必须从一开始就在架构层面中考虑它。如果是事后才做，就需要修改大量的文件，这将导致层出不穷的 bugs。

![redux-redo-undo](../assets/What-Is-Redux-A-Designer’s-Guide/redux-redo-undo.png)

:framed_picture: 撤消、重做 ([Beebee](https://beebeeye.github.io/) 作图)

正因为 Redux 需要每个动作都以文本的形式进行描述，所以可以说是天生就支持撤消/重做。这个[文档](https://redux.js.org/recipes/implementing-undo-history)中介绍了如何使用 Redux 来实现撤消/重做。

### 协作环境

如果你开发的应用类似于 Google Docs ，可以多人协作来完成复杂任务，可以考虑使用 Redux 。它能够为你完成大量繁重的工作。

![redux-google-docs](../assets/What-Is-Redux-A-Designer’s-Guide/redux-google-docs.png)

:framed_picture: Google Docs ([Beebee](https://beebeeye.github.io/) 作图)

Redux 使得通过网络来发送当前用户正在做的事变得很简单。接收到另一个用户在另一台机器上执行的操作后，重放另一个用户所做的更改，并与本地正在发生的动作合并起来是很容易的。

### Optimistic UI

Optimistic UI 是一种提升应用用户体验的方式。它可以使得运行在慢网速上的应用也能快速响应用户的操作。在需要实时响应的应用中，这是一种流行的策略，例如第一人称射击游戏。

![redux-optimisticui](../assets/What-Is-Redux-A-Designer’s-Guide/redux-optimisticui.png)

:framed_picture: Optimistic UI ([Beebee](https://beebeeye.github.io/) 作图)

举个简单例子，在 Twitter 应用中，当你点赞时其实是需要请求服务器来做一些检查的，例如当前推文是否存在。Optimistic UI 的做法不是传统的转圈等待几秒，然后显示结果，而是选择欺骗用户！它事先假定所有请求都是成功的，当用户点赞时直接+1。

![redux-twitter-heart](../assets/What-Is-Redux-A-Designer’s-Guide/redux-twitter-heart.png)

:framed_picture: Twitter 点赞 ([Beebee](https://beebeeye.github.io/) 作图)

这种方式有效的原因在于大多数时候请求都是正常的。当请求失败是，应用只需回滚至前一个 UI 状态即可，并使用服务器响应的实际结果，例如显示错误信息。

如同撤消/重做一样，Redux 也支持 Optimistic UI 。它使得一切都变得简单起来，比如纪录、重放和当请求失败时进行回滚。

### 状态持久化和初始状态加载

Redux 使得将应用中发生的一切纪录并保存下来变得非常简单。就算后面电脑重启，应用也能够轻松加载所有数据，并从完全相同的位置继续运行，就好像它从来没有被中断过一样。

![redux-game-save-load](../assets/What-Is-Redux-A-Designer’s-Guide/redux-game-save-load.png)

:framed_picture: 保存/加载游戏进度 ([Beebee](https://beebeeye.github.io/) 作图)

使用 Redux 开发游戏的话，只需少量代码便能够保存/加载游戏进度，而无需改变游戏本身的代码。

### 真正可扩展的系统

使用 Redux ，你必须 “dispatch” 动作才能更新应用中的数据。这一限制使得我们几乎可以将应用中发生的一切都联系起来。

你可以构建真正可扩展的应用，其中每个功能都可以由用户来自定义。例如，参考 [Hyper](https://hyper.is/#extensions-api) ，这是一个使用 Redux 开发的终端应用。“hyperpower” 插件增加了光标的闪光点，并可以使窗口抖动。你是否喜欢这种 “wow” 模式呢？(或许这功能并没有什么用，但却是足够吸人眼球)

![redux-hyper](../assets/What-Is-Redux-A-Designer’s-Guide/redux-hyper.gif)

:framed_picture: 终端应用 Hyper 中的 “wow” 模式 ([Beebee](https://beebeeye.github.io/) 作图)

### 时间旅行调试

当调试应用时能够进行时间旅行会是怎样一种体验？运行应用的过程中，随意倒退或前进几次以找到 bug 发生的确切位置，修复 bug 后重放以确认是否修复。

Redux 让开发者梦想成真。[Redux 开发者工具](https://github.com/reduxjs/redux-devtools)可以使开发者通过拖拽滑动条来操纵应用的进度，就像 Youtube 视频一般。

它是如何工作的呢？还记得 Redux 的三大原则吗？它们正是秘诀所在。

![redux-time-travel](../assets/What-Is-Redux-A-Designer’s-Guide/redux-time-travel.gif)

:framed_picture: 在 Redux 开发者工具进行时间旅行 ([Beebee](https://beebeeye.github.io/) 作图)

### 自动反馈 Bug

想象一下，用户发现应用中存在问题并想进行反馈。她煞费苦心地回忆和描述她所做过的一切。然后开发人员尝试去手动执行这些步骤以查看 bug 是否复现。用户的反馈很可能是模糊不清的。开发人员很难找到 bug 出现的原因。

如果是这样呢，当用户点击 “反馈问题” 按钮时，系统会自动地将用户本地的状态发送给开发人员。开发人员随即点击 “重发 bug” 按钮便可查看 bug 究竟是如何产生的。Bug 随即被修复，大家都很开心！

[Redux Bug Reporter](https://github.com/dtschust/redux-bug-reporter) 就是这样玩的。它的工作原理呢？Redux 的限制条件让一切变成可能。

![redux-bug-reports](../assets/What-Is-Redux-A-Designer’s-Guide/redux-bug-reports.png)

:framed_picture: 自动反馈 Bug ([Beebee](https://beebeeye.github.io/) 作图)

## Redux 的缺点

Redux 的三大原则其实是一把双刃剑。它开启强大功能的同时也不可避免地带来一些副作用。

### 陡峭的学习曲线

Redux 的学习曲线相当陡峭，需要时间去理解、记忆和熟悉它的模式。如果你完全不会 Redux 和 React ，不推荐你两者同时学习。

### “样板” 代码

在大多数情况下，使用 Redux 就意味着要多写很多代码。通常需要编写多个文件才能让一个小功能运行起来。开发者一直都在抱怨使用 Redux 时所编写的“样板”代码。

我也知道，这听起来很矛盾。但我可曾说过 **Redux 使用很少量的代码就可以实现这些功能？**这就有点类似于使用洗碗机。首先，你得花时间仔细地排列盘子。直到完成洗碗你才感受到洗碗机的好处，它节省了洗碗、清洗餐具等方面的时间。所以需要你来决定这个准备时间是否值得!

### 性能损耗

Redux 的三大原则会对性能产生一些影响。每当数据发生变化时，它会增加一点性能开销。在绝大多数情况下，这不是什么大问题，性能损耗也不明显。但是，当 store 中有大量数据并且数据变化频率非常高的话(例如用户在移动设备上频繁地打字)，UI 可能会变得卡顿。

## 加分项: Redux 不只是为 React 而生

一种常见的误解就是 Redux 只是为 React 提供的，如果离开了 React ，Redux 将一无是处。确实，正如我们之前一直所讨论的，Redux 解决了 React 的一些痛点。React 是最最常见的 Redux 用例。

但是事实上，Redux 可以和任何前端框架一起使用，比如 Angular、Ember.js，甚至是 jQuery 或原生 JavaScript 。试试 Google 一下，你会发现 [这个](https://github.com/angular-redux/ng-redux)、[这个](https://github.com/ember-redux/ember-redux)、[这个](https://github.com/mkamakura/redux-jquery)，甚至是[这个](https://www.sitepoint.com/redux-without-react-state-management-vanilla-javascript/)。Redux 的思想可以应用于任何地方
！

随着你越来越广泛地使用 Redux ，你可以在多种场景下享受它带来的好处，而不仅仅是在 React 应用中。

![redux-work-well-with-other](../assets/What-Is-Redux-A-Designer’s-Guide/redux-work-well-with-other.png)

:framed_picture: Redux 可以搭配其他前端框架一起使用 ([Beebee](https://beebeeye.github.io/) 作图)

## 总结

作为工具，Redux 自身也需要去权衡。它开启强大功能的同时也带来了一些不可避免的缺点。一个开发团队的职责就是进行评估，看如何进行取舍并作出明智的选择。

作为设计师，如果我们能够理解 Redux 的优缺点，我们就能够从设计的角度为此决策作出贡献。举个例子，或许我们设计出的 UI 能够缓解潜在的性能影响？也许我们可以提倡使用撤消/重做功能来替代大量的确认对话框？或许我们可以提倡 optimistic UI ，因为它能够以相对较低的代价来提升用户体验。

理解技术的好处与局限性，并作出相应的设计。这正是我对斯蒂夫·乔布斯的名言 “设计关乎于工作原理。” 的解读。
